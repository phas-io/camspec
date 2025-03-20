---
title: Home
layout: home
nav_order: 1
---
# The CAMSPEC file format

In advanced manufacturing systems, it is sometimes necessary to ensure consistency in the manufacturing parameters of parts. That is to say, over the course of years, a manufacturer may produce a file many hundreds or thousands of times, sometimes at intermittent intervals. In order to ensure consistency of part properties, the manufacturing parameters (such as orientation, etc) need to be maintained. Unfortunately this is often extremely difficult in practice, because order management software is separate from CAM software.

The CAMSPEC format (.cams/.camspec) is a file format designed to communicate manufacturing constraints and requirements between order management systems and Computer Aided Manufacturing (CAM) software. It enables precise specification of part orientation, positioning, and manufacturing constraints for multiple manufacturing processes.

JSON schema available here: `https://camspec.org/schema/1.0.0/camspec.json`

The [specification is under the MIT license](https://camspec.org/LICENSE.md). It is available for both commercial and non-commercial usage. 

The CAMSPEC format was initially developed by [Phasio](https://phas.io) and is currently maintained by the Phasio team.
