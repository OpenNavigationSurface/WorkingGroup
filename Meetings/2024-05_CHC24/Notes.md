# Open Navigation Surface Working Group Meeting Summary

These notes were taken during the ONSWG meeting at the Canadian Hydrographic Conference in St. John's, NFL on 2024-05-30; they are the amalgamation of notes by Calder and Miles, with additions from subsequent review of the meeting audio.

The primary source for this document is the [Working Group's repository](https://github.com/OpenNavigationSurface/WorkingGroup).  An additional copy of the static MarkDown is posted in the [Discussions](https://github.com/OpenNavigationSurface/WorkingGroup/discussions) section of the repository associated with the meeting.  In case of discrepancy, the repository shall take precedence.

In the following, particular opinions or contributions are indicated with names in **bold**, and action items are called out in <span style="color:red;">red</span>.  The acronyn "CR" means "Candidate Release" (i.e., a release of the library for comments) and "FR" means "Full Release".  Links to the Working Group's [GitHub](https://github.com/OpenNavigationSurface/WorkingGroup) repository issues tracker are provided for reference.

## Summary of the Discussion

### Uncertainty Model Definition

The group discussed the [proposal](https://github.com/OpenNavigationSurface/WorkingGroup/issues/2) from NOAA to add a new uncertainty descriptor to the controlled-vocabulary list specified in the BAG metadata schema.  Starting with a review [**Buesseler**], the group discussed the implications of adding this to the schema; [**Wilson**] confirmed that this only related to product uncertainty (i.e., for a finalized BAG file) and that it was specific to NOAA products (although other HOs could of course use it).  The significant difference in approach is that the metadata tag is supported by the referenced proposal, which also describes how the values in the mandatory uncertainty layer should be computed.  Previous uncertainty metadata tags have been much looser with their definitions, which has led to variant implementations.

In discussion, there was a request [**Masetti**] to choose a specific name for the definition as part of the implementation, so the intent is clear, and an observation [**Riley**] that it might only be appropriate for MBES data, due to the method of computation.  (It was noted [**Calder**] that spatially-variable metadata is available as an extended layer if this becomes necessary.)  The basic requirement, however, is that the value has to be something that the user can rely on and safely interepret [**Rice**] which is something that the BAG file does not strongly guarantee.

An auxiliary question [**Masetti**] was whether the process of maintaining the definition document for a given metadata tag would be held by the proposer, or taken on by the WG.  Either was considered possible, but while the preferred method might be to have it part of the BAG description, it would probably be a bad idea for the WG to manage NOAA's process [**Calder**].  For the time being, therefore, the consensus appears to be that the proposer should manage their own definition, and the WG intends that any new uncertainty metadata tag means "whatever the defining document says", with the implication that the proposer has to keep track of when changes were made to the definition so that users (typically compilers) would know how to interpret the uncertainty.  A further suggestion [**Renoud**] was that we should provide, somewhere in the metadata, a DOI or URI for where to find the definition associated with any given uncertainty metadata tag.  It was agreed that this would be possible with the NOAA proposal, given the white-paper exists, and can be placed in a long-term stable location as part of the adoption process.

Finally, there was discussion on whether it might be appropriate to have some different "levels" of uncertainty within the formal definition [**Scheltema**], but it seemed likely that this might leave open the opportunity for confusion as to the precise definition of the meaning of the metadata tag [**Calder**] and therefore that adding another tag (there is no limit to how many can be defined) might be the preferred solution in this case.  The WG would have to determine conditions for when to accept new tags under this model in order to avoid a proliferation of very similar, but subtly different, definitions.

The meeting was polled for approval, and the proposal from NOAA for a new metadata tag for "NOAA Standard Uncertainty" (final name to be determined) was approved *nem. con.*

<span style="color:red;">Action: Add an item to the BAG item tracker for update of the metadata schema for uncertainty [**Calder**], and determine a final name for the specification [**Buesseler**/**Wilson**].</span>

A more general discussion on the fate of the current uncertainty metadata tags then ensued.  The concern [**Calder**] is that they are not currently well defined, and therefore they should preferentially not be used for new products.  There are, however, many products that use them, and therefore it is not practical to fully deprecate them (e.g., mark them for removal from the standard in the future).  (As a clarification [**Riley**] the intent here of deprecation is to mean "not to be used for future products" rather than "no longer valid and liable to go away" [**Calder**]; in ISO19135, "retired" or "superceeded" is often used and may be a viable alternative.)  There was general consensus that the WG could make recommendations that current metadata tags not be used for future products, so long [**Masetti**] as the process was clear, well documented, and widely communicated (e.g., through a conference paper), since it affects many other groups (e.g., S-102, Open Geospatial Consortium, GDAL, etc.).  A concern, however, is that when constructing spatially-varying metadata (e.g., when compositing multiple sources into a single BAG), it might be difficult to give good guarantees of uncertainty when the sources are mixed between newer and older metadata tags [**Lawes**] (it was also pointed [**Rice**] out that BAG files would need to be augmented to allow a metadata tag to be part of the spatially-varying metadata, although S-102 allows this).

A further implication of this discussion [**Rice**] is that there seems to be a need for a better definition of what uncertainty means in a bathymetric product, specifically for navigational purposes.  Although the WG has in the past been focussed primarily of development and maintenance of the BAG specification and library, it might be possible to also consider an expansion of the scope to include this sort of discussion, and for the WG to make recommendations.  There may be other groups [**Riley**] in the S-100 world that could also be approached for input/collaboration on this topic.

The feeling of the meeting was that it was too early to propose a formal action on this topic, but that it could be inter-meeting work, and should be part of the discussion in the next WG meeting.

<span style="color:red;">Action: Add an item to the repository tracker on uncertainty definitions for the next meeting [**Calder**].</span>

### Alignment of BlueTopo/S-102/BAG Spatial Metadata Layer

Although the [IHO S-102](https://iho.int/uploads/user/Services%20and%20Standards/HSSC/HSSC14/HSSC14_2022_05.1D_Rev1_S102_PS_draft_2_1_0_clean_PrimarEdits_6May2022.pdf) product specification has its origins in BAG files, the two specifications have diverged since the first version of S-102 was published.  The group therefore discussed the issues involved in aligning products (see [issue 5](https://github.com/OpenNavigationSurface/WorkingGroup/issues/5)), starting with a background statement [**Rice**] specifically with respect to S-102, BAG, and NOAA's [BlueTopo](https://www.nauticalcharts.noaa.gov/data/bluetopo.html) product, which notes that as gridded products become more common there is a requirement for publishers to ensure that the data's interpretation does not vary as it moves along the product pipeline from capture to delivery.  There is therefore some interest in making sure that there is alignment between the various formats that might be used between capture and publication to avoid confusion.  Further, due to the nature of the products, BlueTopo and the BAG format can be relatively easily adjusted, but the IHO development cycle is a little more involved, and alignment with/of S-102 might be a little more challenging.

The request [**Rice**], therefore, is specifically to consider the spatial metadata layer profiles (as defined in the BAG FSD) and determine whether we need to adjust them to match S-102, or vice versa; and to consider how this might be facilitated.

General discussion followed.  The recommendation for interacting with S-102 WG is to provide feedback through the WG chair [**Masetti**], but that it might not be a good time to do so, since the IHO WG is close to a release of v 3.0.0 of the S-102 specification and therefore will have limited capability to consider modifications.  A major concern, specifically, is that data is often compiled from multiple sources, and therefore may not be complete [**Rice**], and therefore you need to interpolate and then specify clearly what you did and what the user can expect from the result.  The primary concern here is how to make sure that everyone has a common understanding of the attributes pertaining to this process, and therefore that the interpretation of the results of the process are clearly interpreted between producers, vendors, and users.

It is also of concern [**Masetti**] that any modification to the uncertainty metadata tags (per the previous point) are clearly communicated to IHO for inclusion in 3.0.0, since the S-102 product specification has inherited the definitions from BAG, and these are now enshrined in the official IHO dictionary.

The general feeling of the meeting was that there was a requirement for coordination, although due to deadlines with S-102 publication, this is something that should be considered for either inter-meeting work via correspondence, or at the next WG meeting.

<span style="color:red;">Action: Add an item to the repository tracker on coordination between BAG, S-102, and BlueTopo for the next meeting [**Calder**].</span>

### Certification and Digital Signature

The group discussed the utility of certification and digital signatures as part of the BAG specification (see [issue 7](https://github.com/OpenNavigationSurface/WorkingGroup/issues/7)), starting with a use-case statement [**Lawes**] highlighting the concern that submission of final grids is now allowed, but that there is currently no means in the BAG file to verify their integrity and provenance; this therefore becomes a liability question.  There are also no formats (and particularly open source formats) currently that provide for a DSS and therefore could be used as an alternative for final submission of data.  This is also, from the FIG side, a concern about protecting surveyors, in the sense that they have no means to demonstrate, currently, that their digital data (for which they are legally responsible) have not been modified without their knowledge.  It would be good [**Lawes**] to have this facility.

It was noted [**Calder**] that a Digital Signature Scheme (DSS) was a part of the initial design for BAG files, but that functionality for this was dropped during the final development of API 2.0 due to limitations in the available software libraries for DSS, and limited resources to make the required changes.

General discussion ensued.  It was noted [**Calder**] that the IHO now has, as part of [S-100 part 15](https://iho.int/uploads/user/pubs/standards/s-100/S-100_5.2.0_Final_Clean.pdf), a digital signature (and encryption) scheme, which is based on open source software, particularly [openssl](https://www.openssl.org) and implementing [FIPS 186](https://csrc.nist.gov/pubs/fips/186-4/final) for DSS (the NIST standard scheme).  This therefore might be a good source for future development, at least to the extent that it would align BAG practice with emerging IHO policy for S-100 products, particulary S-102.  It was noted [**Lawes**] that tying directly to the IHO certificate chain could be problematic, particularly if different HOs have different trust chains, but it should be possible [**Calder**] to use the same scheme to demonstrate that the BAG DSS was compatible with the IHO model if an HO (or other data generator) wanted to adopt it in practice.  The BAG library could readily provide tools to support this functionality.  It was suggested [**Riley**] that even if the file was not signed, having a secure hash (the first stage of the DSS) stored with the file would give some measure of protection against accidental, or intentional, modification of the file.

There was some concern [**Wilson**] that the effort in applying a DSS should be minimized, or constrained to certain products (e.g., the final deliverable) so that the overhead is not high, and [**Pearson**] that it should be compatible with other constraints on producing agencies (e.g., that it was US Department of Defence allowed for the US Naval Oceanographic Office, among many others).  Given those concerns, however, there was no general objection to moving ahead with reinstating the DSS for BAG files, using the IHO S-100 model as a guide for compatibility.

There being no objection to re-introducing a DSS into the BAG specification, the group disucssed process for moving forward.  The group agreed that the IHO S-100 part 15 specification would constitute a white-paper describing the method, and therefore that the WG could move ahead directly to implementation within the support library, and reintroduce the DSS into the file specification.

<span style="color:red;">Action: Add an item to the BAG repository to re-introduce an IHO S-100 compatible DSS in BAG for a future release. [**Calder**].</span>

### Encouraging More Full Vendor Implementations

There are a number of BAG implementations, but it appears clear [**Lawes**] (see [issue 6](https://github.com/OpenNavigationSurface/WorkingGroup/issues/6)) that not all implementations are equal in the sense that many do not include the "optional" auxiliary layers beyond the mandatory elevation and uncertainty, even though the producing software generates the required information.  A particular concern is that the proprietary formats that are generated by the production software are being used as an alternative to open source solutions, even though this means that the potential to read those files at some remote date (e.g., +10 yrs) is unknown, but likely limited.  The question, therefore, is how to encourage vendors to produce a more full implementation of BAG files, which are much more likely to be readable in the long term and therefore more suitable for archive.

General discussion ensued.  One option [**Lawes**] would be to define a set standard for layers that would be required for a BAG file to be considered a BAG file; this would, however, make these optional layers no longer optional [**Calder**] and therefore make a greater burden on vendors to make BAG files that, perhaps, do not require them (e.g., final products).  Failing mandating optional layers [**Lawes**], it might be enough for the vendors to at least provide the option to populate the optional layers, and then leave it up to the users to decide which to include for their use case; this is therefore a request to the vendors for some additional development.  It was suggested [**Riley**] that one way to do this would be to require the layers as part of the uncertainty specification (e.g., if you say that you are providing "NOAA Product Uncertainty", the file must contain the optional layers that NOAA require to determine that the uncertainty was correctly produced [**Masetti**]).

Vendors in the meeting indicated that this "user demand" might be the best approach, i.e., to make them part of a requirement (e.g., from the HOs) so that the software can not be used for production unless it provides these facilities [**Neville**, **Paton**].  Fundamentally, there needs to be a commercial reason (e.g., loss of business) in order to make the case for expending the engineering resources to create and then maintain this capability.

The question of who pushes this was then raised.  While HOs can do this when it comes time to spend money on software, the WG itself might have a role to play [**Lawes**] as an expert body, since most field hydrographers do not have the time or facilities to push for this sort of accommodation.  Part of the story here is that the people doing the negotiation on behalf of the larger organizations (typically HOs) be better informed of what to ask for, particularly with respect to open source formats that can be safely archived for the long term.  To some extent this is a "chicken and egg" situation: if the file does not support the layers, users will go elsewhere; if the users do not ask for the layers, they will never be added.  It was pointed out that this is generally true [**Rice**] of new technologies, and therefore there needs to be a stalking horse to push the issue with the vendors.  It was indicated that NOAA was interested in this issue [**Riley**] and that they would be willing to assist, so long as we were clear on what to ask for, given the effort that it would take to make this happen.

<span style="color:red;">Action: Advocate for better tools to generate BAG files in commercial software, specifically to allow the user to choose auxiliary data to be written into the file. [**All**].</span>

<span style="color:red;">Action: Consider providing a NOAA specification requirement for auxiliary layers, with careful regard to layers supporting uncertainty specification. [**Riley**].</span>


### Open Geospatial Consortium Community Standard Request

A brief summary of the work done to make BAG an Open Geospatial Consortium community standard was provided [**Rice**].  The Technical Committee [voted](https://portal.opengeospatial.org/index.php?m=projects&a=view&project_id=82&tab=5&subtab=1) [paywall] on 2024-05-19 to approve a new work item for this, and therefore the proposal will now move forward for discussion and hopefuly adoption.

### Library Status

A brief update on use of [OSS Fuzz](https://github.com/google/oss-fuzz) for the BAG library was provided [**Miles**].  This provides for automated testing of the library for things like memory leaks and buffer overruns, and should assist in providing a more secure version of the library in the future.

## Summary of Action Items and Dates

| Person | Action(s) |  Date |
| ------ | --------- |  ---- |
| Calder | Add an item to the BAG project issues to adopt the NOAA uncertainty specification | 2024-07-30 |
| Buesseler/Wilson | Determine a reference name for NOAA uncertainty specification metadata tag | 2024-07-30 |
| Calder | Add an item to the WG issue tracker for discussion of uncertainty tag "retiral" for next WG meeting | 2024-07-30 |
| Calder | Add an item to the WG issue tracker for discussion of BAG/BlueTopo/S-102 coordination for next WG meeting | 2024-07-30 |
| Calder | Add an item to the BAG issue track to reintroduce an S-100 compatible DSS for BAG 2.1.0 | 2024-07-30  |
| All | Advocate whenever possible for better support of auxiliary layers from vendor software | N/A |
| Riley | Consider a NOAA specification requirement that would mandate auxiliary layers for consistency/compatibility | 2024-12-30 |

## Participants

- Bart Buesseler [NOAA/AHB]
- Shannon Byrne [Leidos]
- Brian Calder [CCOM/JHC, Chair]
- Barry Gallagher [NOAA/HSTB]
- Brett Goldenbloome [Leidos]
- Travis Hamilton [Teledyne CARIS]
- Anthony Klemm [NOAA/HSTB]
- Geoff Lawes [Revelare]
- Larry Mayer [CCOM/JHC]
- Giuseppe Masetti [Danish Hydro]
- Brian Miles [CCOM/JHC]
- James Muggah [QPS]
- Danny Neville [Spatialnetics]
- Mark Paton [QPS]
- Samuel Pearson [NAVO]
- Weston Renoud [QPS]
- Glen Rice [NOAA/HSTB]
- Jack Riley [NOAA/HSTB]
- Chrispijn Scheltema [QPS]
- Matt Wilson [NOAA/AHB]
