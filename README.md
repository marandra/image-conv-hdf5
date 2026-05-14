# image-conv-hdf5

Command-line tool (C) for converting images between filesystem
locations and HDF5 containers/groups. Supports reading from and writing
to HDF5, and converting between common image formats.

Originally developed at sciCORE (University of Basel) in 2016 as part
of a microscopy-image processing pipeline; preserved here for
reference.

> The repository was historically named `image-conv-hfd5` due to a
> typo and has been renamed to `image-conv-hdf5`. GitHub redirects the
> old URL automatically.

## Features

- Reads images from the filesystem or from inside an HDF5
  container/group.
- Writes images to the filesystem or into an HDF5 container/group.
- Bulk mode (`-b`): clones a whole HDF5 container, preserving the group
  structure into the output.
- Default output format: JP2 (via OpenJPEG).
- Multi-page TIFF support through the `-p` page selector.
- Conversion runs in a RAM disk for I/O efficiency.

## Dependencies

- [imgcnv](http://www.dimin.net/software/imgcnv/) — general image
  handling.
- [OpenJPEG](http://www.openjpeg.org/) — JP2 read/write.
- HDF5 — container support.

## Usage

```bash
# Plain conversion: filesystem in/out
bimconv_debug [-p page] [-f format] -i /path/to/image [-d /dest/path/]

# Filesystem image → HDF5 group
bimconv_debug [-p page] [-f format] -i /path/to/image [-d /dest/path/] [-O H5/group]

# HDF5 group → filesystem image
bimconv_debug [-p page] [-f format] -I /path/to/H5in/groupin/image [-d /dest/path/]

# HDF5 group → HDF5 group
bimconv_debug [-p page] [-f format] -I /path/to/H5in/groupin/image [-d /dest/path/] [-O H5/group]

# Bulk: clone an entire HDF5 container, converting all images
bimconv_debug [-p page] [-f format] -b H5in [-d /dest/path/]
```

### Arguments

| Flag | Meaning |
|------|---------|
| `-i` | Input path + file. Output keeps the original filename with extension replaced. |
| `-I` | Input path inside an HDF5 container (`path/to/H5in/groupin/image`). |
| `-d` | Output directory. Output keeps the original filename with extension replaced. |
| `-O` | Output target inside an HDF5 container (`H5/group`). |
| `-b` | Bulk mode: input is a full HDF5 container; output is a parallel converted container. |
| `-p` | Page selector for multi-page TIFFs (default: 0). |
| `-f` | Output format (default: `jp2`). |

## Performance

Reference: ~34 seconds for 100 images in a directory on the original
sciCORE workstation.

## Notes

- Metadata strings are written into the JP2 output. OpenJPEG does not
  currently expose JP2 comments on read; as a workaround, Matlab's
  `imfinfo` can retrieve them.
- The tool can be used as a generic image-format converter between any
  two formats `imgcnv` supports, both inside and outside HDF5.

## Status

Developed in 2016. Kept here as a record of the approach.

## License

See `LICENSE`.
