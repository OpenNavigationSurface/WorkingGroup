# Open Navigation Surface Working Group Meeting Summary

These notes were taken during the ONSWG meeting at the Canadian Hydrographic Conference in Montréal, QC on 2026-04-29; they are the amalgamation of notes by Calder and Miles, with additions from subsequent review of the meeting audio.

The primary source for this document is the [Working Group's repository](https://github.com/OpenNavigationSurface/WorkingGroup).  An additional copy of the static MarkDown is posted in the [Discussions](https://github.com/OpenNavigationSurface/WorkingGroup/discussions) section of the repository associated with the meeting.  In case of discrepancy, the repository shall take precedence.

In the following, particular opinions or contributions are indicated with names in **bold**, and action items are called out in <span style="color:red;">red</span>.  The acronyn "CR" means "Candidate Release" (i.e., a release of the library for comments) and "FR" means "Full Release".  Links to the Working Group's [GitHub](https://github.com/OpenNavigationSurface/WorkingGroup) repository issues tracker are provided for reference.

## Summary of the Discussion

### OGC Community Standard Process

Glen Rice gave a short overview of the OGC Community Standard process, confirming that the BAG file format has now been published on the [OGC website](https://www.ogc.org/standards/bag/).  The adoption of the file as a standard means that we now have to pay more attention to making sure that this is kept up to date as new releases are made of the library.

### Library Releases and Downloads

Brian Miles gave updates on the number of downloads of the release through both the Python bindings (13k) and C++ library (2.7k) in the last six months.  He also confirmed the release of version 2.0.5 of the library, which has a number of bug fixes, but also resolves the concern about management of the shape of different arrays in a single file outlined during the [previous WG meeting](https://github.com/OpenNavigationSurface/WorkingGroup/blob/main/Meetings/2025-03_USHydro25/Notes.md).  After examination of example data provided by Shannon Byrne and others, the conclusion is that the likelihood of having files in the wild that used the originally-intended 1D array for variable-resolution refinements was vanishingly small, so it was possible to simply accommodate to the field-observed situation of a 2D array with a single row, simplifying the code.  Details are [available here](https://github.com/OpenNavigationSurface/BAG/issues/109).  Anthony Klemm reported that NOAA has integrated 2.0.5 into their implementations, and initial indications are good, but that more comprehensive testing on variable resolution BAGs is still required.

### Future Directions

A general discussion was then led by Brian Calder, focussed on future development for the project.  An immediate question was our attitude towards use of LLM coding agents in the future development of the project, particularly to assist in building the test coverage for the library.  There was no objection to this idea for code, although Stacy Johnson indicated that anything that touched the data itself would be problematic.  Glen Rice reported that this has also been an active topic of discussion within other FOSS projects (particularly GDAL and QGIS), which might be useful for reference to our discussion, but the general conclusion among those present was that it would be useful to have a position statement on declaration of the use of coding agents on any PR so that the reviewers would be clear on the level of scrutiny required.  A particular concern would be any binary files included in the test suite that might be used to smuggle in a supply-chain attack, or PRs with sufficient code volume to hide something nefarious.

<span style="color:red;">Action: Draft and circulate a position statement on the declaration of the use of coding agents in the development of contributed code in any PR. [**Miles**].</span>

The discussion then turned to security testing of the library, driven primarily by the previously discovered problems through the enrollment of the project in the [OSSFuzz Project](https://bughunters.google.com/open-source-security/oss-fuzz).  Most of the problems found are due to the underlying HDF5 library, and are being addressed as resources allow; Shannon Byrne indicated that it might also be possible to use internal tools to analysis of the codebase in automated fashion, and Stacy Johnson reported that METRON at CNMOC are doing code review that would include the BAG library for another project, and may be able to offer comments.  Mark Paton also suggested that it might be possible to use the [`vcpkg` manager](https://vcpkg.io/en/) as a way to make sure that we have updated libraries when we build, and as a mechanism for ensuring that we take updates to libraries when possible.  Weston Renoud indicated that a Software Bill of Materials might be required for releases of software in the European Union at some point in the (near) future, and that therefore anything that we could do to assist with this for integrators would be a good idea.

<span style="color:red;">Action: Investigate use of automated code analysis techniques (with `Fortify`) for security review of the project code-base. [**Byrne**].</span>

<span style="color:red;">Action: Report any findings from CNMOC review of BAG library to the WG. [**Johnson**].</span>

<span style="color:red;">Action: Investigate use of `vcpkg` for library dependence management. [**Miles**].</span>

Finally, the group discussed the option to use an alternative encoding for the BAG file contents either instead of, or in addition to, HDF5.  This might include encodings that are a little more cloud-friendly (e.g., [Zarr](https://zarr.dev) or [Parquet](https://parquet.apache.org)), streaming-capable (e.g., [CaesiumGS](https://github.com/CesiumGS/3d-tiles)) or something more exotic such as a Discrete Global Grid System such as [IGEO7](https://igeo7.org).  There was no consensus on what encoding might be most useful, and it was also not clear how well the contents of the BAG file would adapt to other encodings, since it is quite heavily embedded with HDF5 in order to keep the design clear and simple.  The group therefore agreed to consider this as an issue informally, and address at the next meeting.

<span style="color:red;">Action: Include discussion on alternative encodings in next meeting. [**Calder**].</span>

## Summary of Action Items and Dates

| Person | Action(s) |  Date |
| ------ | --------- |  ---- |
| Miles  | Draft position statement on use of coding agents in contributed PRs. | 2026-10-01 |
| Byrne  | Investigate use of automated code analysis tools for security review. | 2027-04-01 |
| Johnson | Report any findings from CNMOC review of BAG library. | 2026-10-01 |
| Miles  | Investigate use of `vcpkg` for library dependence management. | 2027-04-01 |
| Calder | Include alternate encoding discussion in next WG meeting. | 2027-04-01 |

## Participants

- Patrick Bradly [Teledyne CARIS]
- Shannon Byrne [Leidos]
- Brian Calder [CCOM/JHC, Chair]
- Brett Goldenbloome [Leidos]
- Vaughn Gooding [Leidos]
- Travis Hamilton [Teledyne CARIS]
- Stacy Johnson [NAVO]
- Anthony Klemm [NOAA/HSTB]
- Brian Miles [CCOM/JHC]
- James Muggah [QPS]
- Mark Paton [QPS]
- Miya Pavlock [NOAA/OCS]
- Weston Renoud [QPS]
- Glen Rice [NOAA/HSTB]
- Matt Wilson [NOAA/OCS]
