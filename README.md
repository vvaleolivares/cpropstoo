# CPROPStoo

Welcome!

This is an IDL package to aid in the analysis of emission line data
cubes. It expands and modularizes the CPROPS package introduced by
Rosolowsky & Leroy (2006). It provides utilities to estimate noise and
mask cubes, identify local maxima, partition cubes into object
assignments, and derive properties from object assignments.

If you use this version, please reference Rosolowsky & Leroy
(2006). An additional updated reference is forthcoming in 2014 or
2015. If you would like to contribute to the code, please feel
encouraged to do so via pull request or email aleroy@nrao.edu about
being added as a collaborator on the wiki. Thanks for stopping by!

### Prerequisites

The IDL Astronomy User's library is required to run the package and
the Coyote Graphics libraries are a nominal (though currently unused)
requirement. If you encounter any other dependencies, please email and
we will endeavor to include the routine in this distribution.

### Examples

A suite of rich examples remains under development. Currently the
repository includes one full example applying the full analysis flow
to an unnamed ALMA data cube. See:

* cpropstoo_example : a documented rich example application

More examples forthcoming as papers using the code are accepted.

### User Tasks

These are high level tasks are intended to be accessed by the user. A
normal cube analysis path calls them in sequency. They are described
in approximate order of the workflow for a normal application of
CPROPS.

###### Units and Book Keeping

* cprops_check_header : verify that a cube is appropriate for
  analysis: units are K, km/s, and has beam info in header.

* prep_cube : attempt to place a cube in correct units. This can be
  hit-or-miss but will save you some annoying work if it fires off
  correctly.

###### Signal Identification

* make_noise_cube : accept a cube and optionally a mask and return 0,
  1, 2, or 3d noise estimates.

* make_cprops_mask : makes a mask based on joint thresholding.

###### Feature Identification

* find_local_max : accept a cube and mask and return a set of local
  maxima.

* assign_clfind : accept a list of kernels and a cube and use the
  CLUMPFIND approach (nearest neighbors) to generate an assignment
  cube.

* assign_cprops : accept a list of kernels and a cube use the CPROPS
  approach (unique associated isocontours) to generate an assignment
  cube.

* assign_from_levelprops [documented] : accept a "level moments" style
  structure (generated by level_moments in the dendrogram work flow)
  combined with an array of flags and map this back to a cube
  assignment giving the largest "flagged" structures (e.g., the
  largest bound structures).

###### Feature Characterization

<em> The feature characterization distinguishes "moments" - which are
defined loosely as values calculated from the cube with little
awareness of metadata (e.g., sizes in pixels) - and "properties" -
which are more physical values (e.g., deconvolved angular or linear
sizes). <\em>

* cube_to_moments : extract moment measurements for a list of clouds
  given an assignment cube and a data cube

* cube_to_level_moments : measure moments for each kernel plus contour
  combination in a cube. Feeds dendrogram analysis or other multiscale
  approaches.

* moments_to_props : calculate cloud properties based on moment
  measurements and other physical information.

* extract_dendro : extract a tree diagram from the level moments type
  structure.

###### Analysis of Results

###### Monte Carlo

* add_noise_to_cube : add correlated noise to a data cube. Useful for
  Monte Carlo calculations.

### Lower Level Routines

These programs are called by the higher level routines. They may still
be of general use.

<em> These are alphabetized by topic. <\em>

###### Astrophysical or Observational Calculations

<em> We have found these widely useful. <\em>

* calc_jtok [documented] : convert from Janskies per beam to Kelvin.

* deconvolve_gauss : deconvolve one Gaussian from another. Rigorously
  useful for interferometer beams. Approximately useful for clouds.

* make_axes [documented] : generate RA, Dec, and Velocity axes or
  images from FITS headers.

###### Cube Infrastructure

<em> Note that these are potentially of general use for moving fluidly
between cube data and vectorized analysis. </em>

* cubify [documented] : convert a vector of {x,y,v,t} measurements into a cube

* ind_to_xyv : convert cube index to pixels

* vectorify [documented] : convert a subset of a cube into {x,y,v,t} vector

* xyv_to_ind : convert pixels to cube index

###### Display

<em> A long term goal would be to deprecate these in favor the Coyote
graphic libraries. But the overhead in getting the disp functionality
from the cg routines may preclude ever actually doing that. </em>

* counter [documented] : progress-bar style counter

* disp [documented] : two-d image display program.

* fasthist [documented] : quick histogram program.

###### Feature Characterization Infrastructure

* calc_props_from_moments: convert moment measurements to properties.

###### Math

* bisection [documented] : bisection root finder

* erf0 [documented] : helper function allowing the root finder to
  solve for the error function at a particular x value.

###### Moment Calculation

* calc_gauss_corr : procedurally generated file that calculates the
  aperture corrections for a three dimensional Gaussian. Created by
  gauss_model.

* ellfit [documented] : fit a two dimensional ellipse to data, solving
  for the principle axes.

* extrap [documented] : use a curve-of-growth analysis to derive the
  correction needed to extrapolate a moment to perfect sensitivity.

* gauss_model [documented] : generates a program or IDL file recording
  the aperture corrections appropriate for a 3-d Gaussian as a
  function of peak-to-edge ratio.

* measure_moments : given a vector x, y, v, and t calculate a
  structure containing moments, wrapping the extrapolation

* pa_moment : use PCA to find suggest the major and minor axis

###### Peak Identification

* alllocmax : find all candidate local maxima via rolling a cube.

* decimate_kernels : reject candidate local maxima

* mergefind_approx : solve for merger levels among a set of kernels

* write_kernels : write a set of kernels to a text or IDL file

###### Properties and Moment "Packages"

<em> Feature characterization is grouped into "packages" that
calculate families of related moments and properties. These packages
each do three things: return an empty structure of moments/properties,
calculate moments, and convert moments to properties given proper
metadta. <\em>

* moments_classic : the original Rosolowsky & Leroy '06 moments.

* moments_gausscor : properties calculated using Gaussian
  extrapolation.

* moments_area : properties calculated using area as a size metric.

###### Signal Identification

<em> These routines are of general use dealing with binary masks. <\em>

* grow_mask [documented] : manipulate masks.

* stat_mask : extract statistics (volume, area, and extent along the v
  axis) on regions inside a mask.

###### Structure Handling

* add_props_fields [documented] : add fields to a structure related to property
  measurements

* alphabetize_struct [documented] : alphabetize the fields in a
  structure.

* empty_moment_struct [documented] : initialize an empty moment structure.

###### Vector Analysis

<em> Note that these are potentially of wide general use. </em>

* contour_values : return contours given data and some criteria.

* mad [documented] : median absolute deviation. Cheap, robust noise estimate.
