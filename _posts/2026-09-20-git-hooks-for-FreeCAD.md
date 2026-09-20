---
layout: post
title: Pre-commit hooks for FreeCAD
---

With the release of FreeCAD 1.1 and upcoming increased pace of releases, it is
a good time to use FreeCAD for Open Hardware (and personal) projects. One thing
that I have struggled with in my personal projects was keeping the preview images
up to date in the README. I therefore figured it would be good to see what kind
of information we can automatically extract from a FreeCAD file to make a repository
with a FreeCAD file more useful at a glance or for people without FreeCAD installed.

As an intial experiment, I created some simple, but useful git hooks that will
be useful for Open Hardware projects. If you don't care about the details and
just want the essentials, [read the main README.md in the repo containing the
git hooks](https://codeberg.org/NemoAndrea/pre-commit-hooks-for-FreeCAD).

## Goals

We have two concrete goals:

1. Extract a useful and small (in terms of disk use) thumbnail preview to show 
what the FreeCAD file contains. There is a lot of value in a picture.
1. Extract a BOM that quickly shows what parts to be sourced. It should be 
csv for maximum interoperability.

In theory, one could develop the habit to manually take a screenshot of the 
viewport and manually export the BOM as CSV before every commit. But this is
error-prone and can accidentally be forgotten. We want to automate it; ideally
without increasing the repo size on disk (by much).

We will set out to use python, as we will need something more powerful than bash,
and it is something is available on Mac, Windows and Linux anyway.

**Picture Preview**

FreeCAD files already save a small preview thumbnail in the .FCStd file. Since
.FCStd  files are just .zip containers, we can easily extract that from FreeCAD
file, rename it to something like `preview.png` and add it to the repo in a 
pre-commit hook.

This we can easily do with the following python code:

```python
import zipfile
import os

zipfile.ZipFile('<YOURFILE>.FCStd').extract('thumbnails/thumbnails.png')  # extract thumbnail
os.replace('thumbnails/Thumbnail.png', 'preview.png')  # rename file
os.rmdir('thumbnails')  # clean up empty directory
```

Or, when running from the command line, as a oneliner:

```bash
python3 -c "import zipfile; zipfile.ZipFile('<YOURFILE>.FCStd').extract('thumbnails/Thumbnail.png'); import os; os.replace('thumbnails/Thumbnail.png', 'preview.png'); os.rmdir('thumbnails')"
```

The above will work nicely, but now we are including our image twice in the repo;
which is not desireable; even if each image is only a few kb. Instead, we will
delete the file from the original .FCStd file after we have extracted it. 
FreeCAD doesn't mind if that file is missing when the file is opened.[^1] 

This gives us the following basic python code:

```python
import zipfile
import os

file = "<YOURFILE>"  # no extension

# We try to extract a thumbnail image from the FreeCAD file. It may not exist,
# as the user may have disabled the saving of Thumbnails in FreeCAD, or may 
# have reverted a commit, resulting in an unstaged FreeCAD file with the thumbnail
# already extracted.
try:
    zipfile.ZipFile(file+".FCStd").extract('thumbnails/Thumbnail.png')  # extract thumbnail
    os.replace('thumbnails/Thumbnail.png', 'picture.png')  # rename file
    os.rmdir('thumbnails')  # clean up empty directory

    temp_zip_path = file + "_temp.FCStd"

    with (
        zipfile.ZipFile(file+".FCStd", "r") as zip_read,
        zipfile.ZipFile(
            temp_zip_path, "w", compression=zipfile.ZIP_STORED
        ) as zip_write,
    ):
        for item in zip_read.infolist():
            #print(item.filename)
            if item.filename != "thumbnails/Thumbnail.png":
                # copy all other entries intact
                zip_write.writestr(
                    item,
                    zip_read.read(item.filename),
                    compress_type=zipfile.ZIP_STORED  # no compression
            )
    print(f"Extracted thumbnail image from FreeCAD file '{file}.FCStd'")
except KeyError:
    print(f"FreeCAD file {file} does not contain a thumbnail. Check your FreeCAD configuration to make sure that a thumbnail is automatically included with the save file.")

```

We will write up a nicer version once we adress the point below. But for now, 
we have managed to automatically extract a preview image that we can
reference in our README file (or elsewhere), without increasing the repo size,
summoning errors in FreeCAD when we opened the file, or without us having to
take screenshots. FreeCAD will automatically save a new thumbnail image when
it saves the file (and edits have been made). So if we set this up as a commit
hook, we have a stable preview image in our README that always reflects the
latest iteration of the design at no cost.

> Note: If the FreeCAD file hasn't changed, then the pre-commit hook also
> will not generate any new files; the extracted preview.png will be identical to 
> the one extracted in the last commit.

> Note: We must handle the case for FreeCAD files without a thumbnail, and that we will re-save the zip file without any compression; as that is the mode that should be used for FreeCAD files managed by git.

**BOM**

The new Assembly workbench introduced in FreeCAD 1.0 has a really nice dynamic
BOM system (made more capable in 1.1). While we could manually maintain a separate
BOM (which would be lightweight), it would risk getting out of sync with our
latest FreeCAD assembly. We might add a screw, or remove and old part from
our complex assembly, and forget to update it in the BOM! It is therefore 
preferable to have the assembly be the master object that the BOM is generated from.
It means metadata fields (like manufacturer, cost, etc) will need to be specified 
in the FreeCAD objects themselves. But good BOM management is another topic. 
For now, let's start from the premise that we have a FreeCAD 1.1 assembly with
a BOM attached to it. '

We would want to achieve the same goal as our preview image: extract the BOM
to a CSV that we can easily open on any device and nicely track with git. If 
we can do so without increasing the repo size, or having FreeCAD throw errors
when we open the .FCStd file after, that would be nice.

Luckily for us, the BOM is stored in a format that is easy and can (in theory) 
reliably be parsed into CSV without needing FreeCAD itself. It is stored in
`Document.xml` (always present if you unzip a FreeCAD file). The structure is 
something like this:

```xml
<Document>
    ...
        <Object name="Bill_of_Materials">
            <Properties Count="11" TransientCount="0">
                ...
                <Property name="cells" type="Spreadsheet::PropertySheet" status="67108864">
                    <Cells Count="5" xlink="1">
                        <XLinks count="0">
                        </XLinks>
                        <Cell address="A1" content="&apos;Description" />
                        <Cell address="B1" content="&apos;.custom" />
                        <Cell address="B2" content="&apos;a new custom status" />
                        <Cell address="B3" content="&apos;N/A" />
                        <Cell address="B4" content="&apos; cylinder for friends" />
                    </Cells>
                </Property>
                ...
        </Object>
    ...
</Document>
```

We can parse this information to a `.csv`. It also turns out that (in my limited
testing), removing these `<Cell>` entries can be done without causing issues,
provided that one also updates the `Count` attribute on the `<Cells>`. The other
`<Property>` elements contain all the information required to recompute the cells
when the document is opened again. 

So, by extracting the BOM as a CSV and removing it from `Document.xml`, we do
not increase the file size of the repo in any way.[^2] If absolute repo size is
a priority, one could omit the .csv and use the same script; there does not 
appear to be an option to exclude the compiled BOM fields from the .FCStd file
in FreeCAD yet.

## Closing thoughts

Surprisingly, I've never played around much git hooks before. I always just
relied on GitHub actions. After these experiments, I understand why so many
projects rely on them for automation and for standardisation for contributors.
I think refining this can help take some of the tricky management for git projects
out of the hands of users. 

Finally, one of the goals for these hooks was to not increase the repo size. This
is achieved by not increasing the size of the checked out _work tree_. That may
actually be too strict. I haven't tested it, but leaving the thumbnail in the
(uncompressed) `.FCStd` file may not increase the _repo_ size as git compression
will likely be able work very well on that byte-identical content. For the BOM,
I think there is merit in removing the BOM content from the `.FCStd` file as
the XML representation will likely not compress super well against the `.csv`.[^3]  
If, in the future, it turns out that git compression makes removing the duplicated
information from the FreeCAD file pointless from a repo size perspective, then
I will update the hooks to reflect that.

## Configuration

Have a look at the [source repository](https://codeberg.org/NemoAndrea/pre-commit-hooks-for-FreeCAD) for usage instructions. 

[^1]: It just 
means that systems with FreeCAD installed will not show that preview thumbnail
for the file if they have their file explorer set to pictogram mode. I think
this is not an issue, as they can look at the README or at preview.jpg anyway.

[^2]: The actual filesize seems a bit inconsistent between recomputes. I imagine
the XML is a less efficient way to save the information, so you probably save
a bit of repo size in practice.

[^3]: It may still do pretty well if it can match long strings like URLs or
long descriptions between the XML and CSV. Maybe I will run a test sometime
in the future.