# Open Navigation Surface Working Group Meeting Summary

These notes were taken during the ONSWG meeting at the US Hydrographic Conference in Wilmington, NC on 2025-03-19; they are the amalgamation of notes by Calder and Miles, with additions from subsequent review of the meeting audio.

The primary source for this document is the [Working Group's repository](https://github.com/OpenNavigationSurface/WorkingGroup).  An additional copy of the static MarkDown is posted in the [Discussions](https://github.com/OpenNavigationSurface/WorkingGroup/discussions) section of the repository associated with the meeting.  In case of discrepancy, the repository shall take precedence.

In the following, particular opinions or contributions are indicated with names in **bold**, and action items are called out in <span style="color:red;">red</span>.  The acronyn "CR" means "Candidate Release" (i.e., a release of the library for comments) and "FR" means "Full Release".  Links to the Working Group's [GitHub](https://github.com/OpenNavigationSurface/WorkingGroup) repository issues tracker are provided for reference.

## Summary of the Discussion

### Library Changes for Different Shape Descriptors

In previous work, it was found that there were [missing descriptor shape parameters (PR110)](https://github.com/OpenNavigationSurface/BAG/pull/110) from BAG files in 2.0.0, with the code assuming that all layers had the same shape.  This fails, among other reasons, where there are separation layers (e.g., to provide a datum transform), or in the case of the spatial metadata layer (e.g., for NOAA National Bathymetric Source).  Further, in the development of PR110 it was found that the library was broken for VR-BAG (i.e., would not have worked) and that VR-BAGs found in the field (e.g., from the U.S. national archive at NCEI) were not consistent with the [Format Specification Document](https://bag.readthedocs.io/en/master/fsd/index.html).  Specifically, the FSD requires that the variable resolution (VR) refinements are a 1D array in the HDF5 structure, but the archived VR-BAGs were using a 2D array with the first dimension being permanently set to R=1 rows.  While these are functionally identical, they use different API calls for reading, and therefore would require code to first read and check the dimensionality, and then adapt as appropriate, complicating readers.  BAG v1.6 can read and write VR-BAG and uses 1D arrays, BAG v2.0 was broken and could not read and write VR-BAG, and GDAL can not write VR-BAG files.  Therefore, an undetermined vendor library is writing inconsistent VR-BAGs.

It was proposed [**Calder**] that a solution might be simply to adjust the FSD to reflect what was being seen in the field since VR-BAGs are still relatively rare, but in subsequent discussion is was observed [**Byrne**] that [Leidos SABER](https://www.leidos.com/sites/leidos/files/2021-03/PDF%20SABER%20Fact%20Sheet_0.pdf) is generating VR-BAGs using v1.6 of the library, and therefore there are probably a substantial proportion of FSD-compliant VR-BAGs in circulation (although potentially not in public archives) which are variant.

General discussion on a potential solution followed.  Short of modifying every VR-BAG in the U.S. National Archive (and potentially other places) to be consistent with the FSD (which was considered possible [**Rice**, **Klemm**] but not optimal), it was generally felt that the approach would have to be to have the VR-BAG portion of v2.0 support either option for read (with a warning if required), but revert to the FSD-compliant version of a 1D array for VR refinements when writing new files.  Example test data [**Byrne**] is available and can be included into improved test suites for GDAL and the reference implementation.

<span style="color:red;">Action: Update PR-110 to support permissive 1D/2D reading of VR-BAG refinements. [**Calder**].</span>

<span style="color:red;">Action: Provide example VR-BAG files from Leidos SABER for testing. [**Byrne**].</span>

<span style="color:red;">Action: Incorporate example VR-BAG files into routine unit testing suite for BAG v2 releases. [**Miles**].</span>

### OGC Community Standard Process

Glen Rice gave an overview of the current status of the drive to have the BAG file format adopted as an [OGC Community Standard](https://www.ogc.org/how-our-standards-program-works/).  This has been ongoing for approximately two years, working its way through the OGC process, and has resulted in a number of improvements to the documentation for the project as part of the technical peer review from the OGC Technical Committee.  Steve Olson (NOAA) reports that the BAG file format has now been approved as a community standard, and is waiting on a formal announcement (which might take some time).

In subsequent discussion, Jack Riley asked whether there had been any discussion on the GDAL drivers for BAG and how they might be affected by the process.  Brian Calder noted that the only requirement was to demonstrate that at least one implementation existed, for which the reference library was sufficient and GDAL did not come up explicityly.  Brian Miles noted, however, that having BAG as a Community Standard might raise questions of conformance of something like GDAL, which might actually be a benefit.

### Library Status

Brian Miles gave an overview of the current status of the library reference implementation, including the release of 2.0.3 adding [NOAA Product Uncertainty 2.0](https://github.com/OpenNavigationSurface/BAG/issues/112) and 2.0.4 with a bugfix for compatibility with newer versions of libxml2 (2.12+) which had caused problems in validating metadata from older files.  He also reported on work on [fixing memory leaks](https://github.com/OpenNavigationSurface/BAG/pull/122) identified through fuzzing (partly as a collaboration with an undergraduate student team at UNH), and [adding a handler for SIGABRT](https://github.com/OpenNavigationSurface/BAG/pull/126) to more gracefully handle unrecoverable errors.

Miles also noted open [PR 110](https://github.com/OpenNavigationSurface/BAG/pull/110) that addresses the issues with layer shape description discussed [above](#library-changes-for-different-shape-descriptors) with associated [discussion](https://github.com/OpenNavigationSurface/BAG/pull/110#discussion_r1901976151).

### Certification and Digital Signatures

[Previous discussion at a prior meeting](https://github.com/OpenNavigationSurface/WorkingGroup/blob/main/Meetings/2024-05_CHC24/Notes.md#certification-and-digital-signature) brought forward the question of adding digital signatures and certification back into BAG 2.0.  Due to lack of resources, this was dropped in the transition from 1.6 to 2.0, but it is now clear that there is increasing demand for this functionality, and support from IHO in [S-100 part 15](https://iho.int/uploads/user/pubs/standards/s-100/S-100_5.1.0_Final_Clean.pdf) using standard libraries and interfaces.

Brian Calder reported on review of S-100 Part 15, which was agreed at the [2024 meeting](https://github.com/OpenNavigationSurface/WorkingGroup/blob/main/Meetings/2024-05_CHC24/Notes.md#certification-and-digital-signature) as a potential implementation approach.  Use of standard tools (with wrappers) makes this a good approach, and therefore there was a proposal [**Calder**] to generate a proof-of-concept implementation in the 2.1.x range of the BAG library.

<span style="color:red;">Action: Develop a proof-of-concept implementation of S-100 Part 15 digital signature and certificate management for BAG 2.1.x [**Calder**].</span>

General discussion on the scope of the proof-of-concept implementation ensued.  A particular concern was that the technology did not require a signature for every update to the file (which might be burdensome on field units).  It was also noted [**Rice**] that there are new FIPS restrictions on allowable signature algorithms, which might affect allowable implementations, particularly with respect to export controls.  Similar restrictions might affect NAVO too; **Byrne** to check.  Further requests were to ensure that while only a single signature might be verifiable (i.e., the last applied signature), that a history of previous signatures was maintained; and that the implementation be portable across multiple architectures and compilers.  Finally, it was noted that the implementation would need to ensure that exporting a signed file (e.g., via GDAL) would result in the signature verifiably failing.  These were noted for implementation.

<span style="color:red;">Action: Check on potential signature functionality and restrictions for NAVO use/SABER implementation.  [**Byrne**].</span>

<span style="color:red;">Action: Capture implementation requests and notes for proof-of-concept.  [**Calder**].</span>

## Summary of Action Items and Dates

| Person | Action(s) |  Date |
| ------ | --------- |  ---- |
| Byrne | Provide example VR-BAG files from Leidos SABER for testing. | 2025-06 |
| Byrne | Check on potential signature functionality and restrictions for NAVO use/SABER implementation. | 2025-10 |
| Calder | Update PR-110 to support permissive 1D/2D reading of VR-BAG refinements. | 2025-08 |
| Calder | Develop a proof-of-concept implementation of S-100 Part 15 digital signature and certificate management for BAG 2.1.x. | CHC26 |
| Calder | Capture implementation requests and notes for Digital Signature proof-of-concept. | 2025-07 |
| Miles | Incorporate example VR-BAG files into routine unit testing suite for BAG v2 releases. | 2025-10 |

## Participants

- Rachel Bobicks [THSOA]
- Shannon Byrne [Leidos]
- Brian Calder [CCOM/JHC, Chair]
- Brett Goldenbloome [Leidos]
- Travis Hamilton [Teledyne CARIS]
- Anthony Klemm [NOAA/HSTB]
- Brian Miles [CCOM/JHC]
- James Muggah [QPS]
- Weston Renoud [QPS]
- Glen Rice [NOAA/HSTB]
- Jack Riley [NOAA/HSTB]
