---
title: Spherical diffusion for phylogenetic biogeography
author: J. Salvador Arias

lang: en-us
---
## Phylogenetic biogeography and diffusion

### The problem

Given the geographic locations
observed in a set of taxa
and their phylogenetic relationships,
we want to infer the geographic location
of the ancestral species.

If we assume that the process by which species disperse through time
can be described as a dispersal model,
then the inference problem is equivalent
to ancestral character estimation on a phylogeny,
where species locations
are treated as an evolving continuous character.

### Dispersal model

We assume that the species dispersal along the branches
can be generalized by a Brownian motion
in the geographic space.

Under this model,
an initial particle makes a spatial random walk
with a particular diffusion parameter
until a branching event,
when it transforms into two particles
that start in the same location and start moving independently.

### Geographic ranges

Observed geographic ranges
can be interpreted as a cloud of multiple particles.
Then,
we want to estimate all the histories
of all the particles
that result in the current species ranges.

## Brownian motion

### What is Brownian motion

![*Brownian motion* is the random movement
produced when microscopic particles in a medium
collide with other particles.](../../figs/brownian.png)

### Modeling Brownian motion

The diffusion of particles in Brownian motion
follows a normal distribution
in which the variance increases with time:

$$
\sigma ^{2} = 2Dt
$$

where *D* is the diffusion parameter.

### Modeling Brownian motion: location

Then,
under Brownian motion,
the location of a particle at a given time
is a sample from a normal distribution
defined by the diffusion parameter
and elapsed time:

$$
\rho (t_{i})= \rho (t_{i-1}) + N(0,2D\left | t_{i} - t_{i-1} \right |)
$$

Generalizing for any time:

$$
\rho (t)= \rho (0) + N(0,2Dt)
$$

### Modeling Brownian motion: PDF

As the location of a particle
at a given time is the product of a process
with a normal distribution,
the probability density function for a location
after a given time
is just the PDF of the normal distribution:

$$
Pr(\rho (t) \mid D)=\frac{1}{\sqrt{4\pi Dt}}\exp \left ( -\frac{(\rho (t)-\rho (0))^{2}}{4Dt} \right )
$$

### Brownian motion in phylogenetic biogeography

- Phylogeography:
  single points in terminals
  (Lemmon & Lemmon 2008 *Syst. Biol.** **57**:544).
- Taxon history:
  geographic ranges.
  - Centroids.
  - Sampling from terminal ranges
    (Nylinder et al 2014 *Syst. Biol.* **63**:178).
  - Integrating the whole range
    (Quintero et al 2015 *Syst. Biol.* **64**:1059).

### Movement on a plane

In biogeography,
Brownian motion occurs in a geographic space,
so we need to model it not as motion on a line
but as motion over the geographic space.

The simplest way to model geographic space
is by using a plane
(i.e., a flat Earth).
This is usually done
by transforming longitude and latitude
into the X and Y coordinates of a Cartesian plane,
with distances measured
using Euclidean distances.
In such a case,
we can model the Brownian motion in several ways:

- Using radial rings (radial distances).
- Uncorrelated motion between axes.
- Correlated motion among axes.

### Movement on a plane: rings

If we only model the distance,
the motion is only governed by a single diffusion parameter,
and we measure the probability of falling
in an infinitesimal radial ring as:

$$
Pr(\rho (t) \mid D)= \left ( \frac{\rho (t)}{2Dt} \right )\exp \left ( -\frac{\rho (t)^{2}}{4Dt} \right )
$$

This is the model proposed by Lemmon and Lemmon
(2008 *Syst. Biol.** **57**:544).
To correct for the sphericity of Earth,
they propose to replace the Euclidean distance of the coordinates,
by the great circle distance.

*They even propose a model test
to select among Euclidean distances
vs. great circle distances!*

### Movement on a plane: uncorrelated motion

An alternative is to model the Brownian motion
independently on each coordinate axis
(Quintero et al 2015 *Syst. Biol.* **64**:1059),
and then a univariate Brownian motion is fitted
for each coordinate.
If both diffusion parameters are equal,
this is the same as the use of rings.

It has the perceived advantage
that we can separate between longitudinal movement
(zonal movement)
and latitudinal movement
(meridional movement).

### Movement on a plane: correlated motion

Use a multivariate normal to take into account
a correlated motion between the latitude
and longitude.
As far as I know,
it was not attempted in phylogenetic biogeography.

### Motion on a plane: problems

As Earth is not flat,
the use of a planar motion model
has several problems:

- Euclidean distances between coordinates
  are not meaningful distances.
- Distances around the poles will be severely distorted.
- Results depend on the reference frame,
  changes in the coordinate system
  will change the results.
- Size of longitude degrees
  is not comparable at different latitude rings.
- The calculated densities
  are not really a probability
  as they are not integrated over the surface of the Earth.

### Motion on a sphere

Instead of using a plane,
a better model can be a spherical Earth.
While Earth is not a perfect sphere,
it can be reasonably approximated by a spherical model.
There are several forms of modeling diffusion on a sphere:

- 3D Euclidean.
- Ring based.
- Wrapped normal.

### Motion on a sphere: 3D Euclidean

Assuming the center of the Earth as the origin
of a 3D Cartesian space,
we can transform latitude and longitude
into X,
Y,
and Z coordinates.
Then we can model a Brownian motion
independently in each coordinate,
assuming that the diffusion parameter
is the same on each axis
(O'Donovan et al 2018 *Nat. Ecol. Evol.* **2**:452).

In the original formulation,
the probability was not scaled with the Earth's surface,
but it can be done,
and the resulting probability function
will be equivalent to the von Mises-Fisher distribution.

### Motion on a sphere: Euclidean problem

The main problem with the Euclidean model
is that it uses the Euclidean distance
between points,
and then the distortion relative
to the great circle distance
will increase with the Euclidean distance.
In fact,
the maximum distance using the Euclidean model is *2r*,
while in the great circle distance,
the maximum distance is $\pi$*r*.
It also models impossible paths
that cross the Earth's interior.

### Motion on a sphere: rings

If we only take into account the great circle distance
between the points,
we can model the diffusion
using the approximation of Ghosh et al
(2012 *E.P.L.* **98**:30003),
as was done by Bouckaert
(2016 *PeerJ* **4**:e2406)
and Louca
(2021 *Syst. Biol.* **70**:340):

$$
Pr(\theta \mid \tau )=\frac{N(\tau )}{\tau}\sqrt{\theta \sin \theta }\exp \left ( -\frac{\theta ^{2}}{2\tau } \right )
$$

Where $\theta$ is the great circle distance
(in radians)
between the two points $\tau =2Dt/r^{2}$,
and $N(\tau)$ is a normalization constant
that must be determined empirically
to make the integral of the density equal to 1.

### Motion on a sphere: wrapped normal

Another approximation,
is to wrap a normal distribution on a sphere
(Hauberg 2018 *FUSION 2018*:704):

$$
Pr(x \mid \mu, \lambda ,t)\propto \exp \left ( -\frac{\lambda }{2t}\theta(x,\mu) ^{2} \right )
$$

where $\lambda$ is the concentration parameter,
which is analogous to the $\kappa$ parameter
of the von Mises-Fisher distribution,
and $\mu$ is the starting point.
This function must be numerically integrated to sum 1.

This approach is the one used by `PhyGeo`.

### Motion on a sphere: limits

The major limit of the spherical normal
is that it does not allow a general form
for uncorrelated or correlated motion.
That is because the concentration matrix
of the multivariate spherical normal
is defined for geodesic lines
that cross the location.
Then,
the movement of the point
will change all the geodesics,
rendering the concentration matrix undefined.

## Likelihood calculation

### Likelihood formula

To calculate the likelihood,
we must take into account all possible locations
of the ancestors
as well as all observed locations.
As the geographic space is continuous,
the likelihood of the tree given the observed locations
is the integral:

$$
Pr(D \mid T, \lambda)= \int_{l_{n+1}} \cdots \int_{l_{root}}\left [\prod_{i=n+1}^{root-1}f(l_{a(i)\rightarrow l_{i}} \mid \lambda,t(b_{i})) \right ]\pi(l_{root})dl_{n+1} \cdots dl_{root}
$$

where $l_{i}$ is a spatial location of a node,
*f* is the spherical normal.

### Likelihood approximations

This integral is intractable,
then it is necessary to approximate it
with numerical methods.

- Independent contrasts.
- Data augmentation:
  - Assignation with the maximum likelihood.
  - MCMC storing mean points.
  - MCMC with a sampling step for ancestors.
- Pruning with a discrete approximation.

### Independent contrasts

Louca
(2021 *Syst. Biol.* **70**:340)
adopts the independent contrast of Felsenstein
(1985 *Am. Nat.* **125**:1),
making independent contrast based on pairs of terminals.
Unlike the planar independent contrast approach,
internal nodes cannot be compared
because the spherical coordinates cannot be added.

This method is quite fast,
but we lost the inference on the internal nodes.

### Data augmentation: Maximum likelihood ancestors

In the approach proposed by Lemmon & Lemmon
(2008 *Syst. Biol.** **57**:544),
a data augmentation approach is used,
assigning explicit locations to the nodes,
and then a search for the locations that maximize the likelihood
is performed using a hill climbing algorithm.

The main problem with this approach
is that only a subset of all possible histories
is used for the inference.

### Data augmentation: MCMC approaches

Other authors use data augmentation
to calculate the likelihood of a particular assignment
and use MCMC integration
under a Bayesian framework
to calculate the integral of the full likelihood.

The most direct approach
is to use a Metropolis-Hasting step for internal nodes
(Bouckaert et al 2012 *Science* **337**:957;
O'Donovan et al 2018 *Nat. Ecol. Evol.* **2**:452)
sample the ancestors using a Metropolis-Hastings step.
On the other hand,
Bouckaert
(2016 *PeerJ* **4**:e2406)
noticed that using mean locations at nodes,
scaled by the branch lengths,
provided a good approximation of the maximum likelihood assignment
(as used by Lemmon & Lemmon)
and use that assignment
to skip the Metropolis-Hasting step for internal nodes.

While these approaches
eventually produce the integral of the likelihood,
they require large samples of the values
for internal nodes to provide a good approximation
of the integral for all histories.

### Pruning algorithm

If we use a pixelation,
we can transform the integrals of the likelihood
into sums:

$$
Pr(D \mid T, \lambda)\approx \sum _{l_{n+1}} \cdots \sum _{l_{root}}\left [\prod_{i=n+1}^{root-1}f(l_{a(i)\rightarrow l_{i}} \mid \lambda,t(b_{i})) \right ]\pi(l_{root})
$$

This problem is the same one
solved by Felsenstein
(1981 *J. Mol. Evol.* **17**: 368)
with the pruning algorithm.

As diffusion is scaled with time,
we do not need to use matrix exponentiation
(which will require a huge amount of memory for a pixelation)
or simulation
(which will make the analysis slow)
to estimate the transition probabilities between pixels.

## Extending the model

### Terminals

So far,
terminals have been modeled as single points.
But the geographic distribution range of a terminal
is usually formed by a collection of locations.
There are two main solutions to this problem:

- Sample the terminal locations using a MCMC step
  (Bouckaert et al 2012 *Science* **337**:957;
  O'Donovan et al 2018 *Nat. Ecol. Evol.* **2**:452).
- Directly integrate the whole distribution range
  (Quintero et al. 2015 *Syst. Biol.* **64**:1059).

### Terminals: likelihood of a terminal

If we define the geographic range as R, the likelihood of a terminal range given its ancestor is
(Quintero et al. 2015 *Syst. Biol.* **64**:1059):

$$
g(R \mid l_{a(i)}, \lambda ,b_{i}(t))=\frac{1}{R} \int_{l_{i\in R}}f(l_{a(i)\rightarrow l_{i}} \mid \lambda,t(b_{i}))dl
$$

### Terminals: pixels

Using a discrete pixelation, the integral became
(Bouckaert et al 2012 *Science* **337**:957):

$$
g(R \mid l_{a(i)}, \lambda , b(t_{i}))=\frac{1}{pix(R)}\sum_{l\in R}f(l_{a(i)\rightarrow l_{i}} \mid \lambda,t(b_{i}))
$$

where *pix* is a function that returns
the number of pixels in the range.

Note that this function is just an application
of the pruning algorithm,
in which the probability of the pixels is scaled
by the observed area of the terminal range.
In `PhyGeo`,
the probabilities of each pixel in a terminal
are scaled to add 1 to all terminals.

### The landscape

The spherical normal is homogeneous,
which means that it assumes that all locations are equivalent.
But we know that not all areas are suitable
for an organism.
We want to take into account the landscape,
so it will be more likely to move to a suitable area
than to one that is unsuitable.

### The landscape: Euler method

Maybe the most correct approach
is to use the Euler method as described by Bouckaert et al
(2012 *Science* **337**:957).
The main drawback of this approach
is that it is based on numerical integration
with very small steps,
which require a lot of memory for the storage of results,
a lot of time for the calculations,
and a drastic reduction of the geographic scale.

### The landscape: condition of suitable pixels

A more simple approximation,
used by `PhyGeo`,
is to use large time steps
and approximate the effect of landscape
by scaling the probability of arrival to a pixel
by a prior of a particular landscape,
and that probability is normalized
by the sum of all possible scald arrival probabilities:

$$
f_{land}(x\rightarrow y \mid \lambda , t)\approx \frac{f(x\rightarrow y \mid \lambda , t)w(y)}{\sum_{j}f(x\rightarrow j \mid \lambda , t)w(j)}
$$

where *w* is the prior of each pixel landscape.
The landscape is retrieved from a landscape pixelation,
and the priors are defined beforehand by the user.

### The landscape: time steps

The scaling of the landscape depends on time,
so it is important to take into account some time steps.
If we use many small-time steps,
the approximation used here
will be very similar to the Euler approach.
But that will be too time-consuming.
Rather,
here we use more broad time stages,
which allow for the influence of the landscape
without an excessive time penalty.

### Time stages

We will use time stages,
which will allow us to introduce some nodes
in the long branches;
these nodes will only have one descendant.
With this approach,
we can continue using the pruning algorithm
instead of calculating explicit dispersal matrices,
which will take a lot of memory.
The drawback is that we will not be able
to reuse the dispersal matrices for many branches.

At the moment,
it is unknown if storing explicit matrices
will produce faster calculations;
the calculations require matrix multiplications
that consume a lot of memory
that is currently unavailable.

### Plate motion models

An advantage of using time stages
is that we can connect those time stages
with the time stages
used in a plate motion model.
Then we can take into account the continental drift
during the inference.

To do this,
at each change in a time stage,
we move the conditional likelihood of the pixels
from their younger locations
to their older locations.

### Plate motion models: special cases

- When several different pixels
  have the same destination,
  the maximum value will be stored
  (i.e., we do not sum the likelihoods).
- When a pixel has different destinations,
  all destinations will have the same likelihood
  (i.e., we do not distribute the likelihoods).
- If a pixel does not exist in the previous time stage,
  the likelihood vanishes.

These procedures are used
to keep the likelihood surface smooth.
It is important to note that in current models,
the number of pixels that fall in these cases
is relatively small.

## Stochastic mapping

### Estimation of ancestral ranges

So far,
we are calculating the likelihood of a reconstruction.
If we store the conditional likelihoods,
we can make estimations of the ancestral states of each node.

Here,
we are interested in the ancestral range
of each internal node.
As we do not have a model of range size evolution,
we cannot estimate the whole ancestral range.
But we can estimate the part of the range
that gives rise to the current observed locations.

### Ancestral estimation in  `PhyGeo`

There are several ways to estimate ancestral ranges.
In `PhyGeo`,
we use a stochastic version of the Yang procedure
(1995 *Genetics* **141**:1641).
As most of the time we are working with time stages,
the procedure is a form of stochastic mapping
(Nielsen 2002 *Syst. Biol.* **51**:729)
for a continuous character,
so here we call it stochastic mapping.

### Root assignment

In each replication of stochastic mapping,
we simulate a single particle
that starts at the root using the conditional likelihoods
and pixel priors at the root.

$$
Pr(l_{root} \mid D, \lambda)=\frac{L_{root(i)}\pi (i)}{\sum_{j} \pi (j)L_{root(j)}}
$$

### Assignment in internal nodes

At internal nodes,
the particle starts from the location of its ancestor,
and its destination is selected by the diffusion function
conditioned by the conditional likelihood
for each pixel in the node:

$$
Pr(n(i) \mid a_{n}(j), D, \lambda)=\frac{L_{n(i)}f(j \rightarrow i \mid \lambda, t_{n})}{\sum_{k}L_{n(k)}f(j \rightarrow k \mid \lambda, t_{n})}
$$

### Assignment in internal nodes: descendants

If the node is a change in time stage,
then the particle will be moved to a new location
given the plate motion model.
If there are several possible destinations,
one is picked at random,
taking into account the pixel prior in the youngest time stage.

If the node is a bifurcation,
then both descendants inherit a particle in the same location.

### Advantages of stochastic mapping

- It is relatively fast
  compared with a full ancestral state estimation.
- As it samples possible histories,
  additional inferences can be made,
  such as potential dispersal routes,
  dispersal speed,
  and time of arrival at a particular location.

### Smoothing

As the ancestral ranges estimated
with stochastic mapping are a product of sampling,
it is useful to have a way to smooth the results
and provide a better view
of the posterior distribution of the ancestral range.

In `PhyGeo`,
this can be done using a kernel density estimation
based on a spherical normal.
This has the advantage of using a spherical function,
but the disadvantage of having to select a $\lambda$ value
for the smoothing.

## Limits

### What have we left out?

- We cannot model cladogenetic events
  (sympatric speciation is assumed
  in all cladogenetic events).
- We do not model the size of the ancestral range
  (no extinction or range expansion).
- We cannot separate the ancestral range
  from model uncertainty
  (was the range broad?
  or our inference is ambiguous?).
- We are limited by the terminals
  that we are able to sample.

### Things that seem difficult at the moment

- *Simultaneous analysis*,
  such as dating and tree inference.
  The method is slow.
- *Ancestral ranges taking tree uncertainty into account*.
  I do not know how to produce a meaningful summary
  as geography changes with time.
- *Comparative biogeography analysis*.
  I do not know how to compare several dispersal routes.
