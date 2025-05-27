# Open Navigation Surface Working Group

**Time:** 0930-1030 EDT Wednesday 19th March, 2025.

**Location:** Room MR 107, Wilmington Conference Center, Wilmington, NC

## Introduction

The Open Navigation Surface Working Group meeting will be held in conjunction with the US Hydro Conference 2025 in Wilmington, North Carolina, United States, details as above.  There are two decisions for the group (majority vote of those present), two informative topics (updates on current activities), and one topic for general discussion (future work items) as outlined below.

## Decisions

1. How to handle library change for [missing descriptor shape parameters](https://github.com/OpenNavigationSurface/BAG/pull/110) (Calder)

2. [Treatment of optional metadata layers in BAG as an OGC standard](https://github.com/OpenNavigationSurface/WorkingGroup/issues/6) (Lawes)

## Informative

1. [OGC Community standard process.](https://github.com/OpenNavigationSurface/WorkingGroup/issues/11) (Rice)

- Provide update on OGC Community Standard status

2. Library status (Miles)

- Releases:
  - 2.0.3: Added metadata enumeration element for [NOAA Product Uncertainty 2.0](https://github.com/OpenNavigationSurface/BAG/issues/112).
  - 2.0.4: Bugfix release to add compatibility with newer versions of libxml2 (2.12+)

- Fuzz testing:
  - [Fixed memory leaks](https://github.com/OpenNavigationSurface/BAG/pull/122) identified through fuzzing
  - [Add handler for SIGABRT](https://github.com/OpenNavigationSurface/BAG/pull/126) to more gracefully handle unrecoverable errors

- Open pull request(s):
  - [layer descriptor shape parameters](https://github.com/OpenNavigationSurface/BAG/pull/110)
    - Addresses issue [109](https://github.com/OpenNavigationSurface/BAG/issues/109)
    - Decision to make:
      - BAG FSD states that VR refinement layer should be a 1D array, however, there are VR BAGs in the wild with 2D VR refinement layers (though with shape [1, N] so 1D data stored in a 2D array).
      - Should we change the FSD and reference implementation to use a 2D structure? This appears to be the path of least resistance.
      - Relevant discussion is [here](https://github.com/OpenNavigationSurface/BAG/pull/110#discussion_r1901976151) 

## Discussions

1. Certification and Digital Signature. (Calder)
