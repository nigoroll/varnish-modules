THIS BRANCH TRACKS VARNISH MASTER!
==================================

While the master branch of the original Varnish Software Repo
https://github.com/varnish/varnish-modules tracks some release (4.1 at
the time of writing), the master branch of this repository acutally
tracks the master branch of
https://github.com/varnishcache/varnish-cache


Varnish module collection by Varnish Software
=============================================

This is a collection of modules ("vmods") extending Varnish VCL used for
describing HTTP request/response policies with additional capabilities.

Included:

* Simpler handling of HTTP cookies
* Variable support
* Request and bandwidth throttling
* Modify and change complex HTTP headers
* 3.0-style saint mode,
* Advanced cache invalidations, and more.

This collection contains the following vmods (previously kept individually):
cookie, vsthrottle, header, saintmode, softpurge, tcp, var, xkey

Installation
------------

Installation requires an installed version of Varnish Cache, including the
development files. Requirement can be found in the `Varnish documentation`_.

.. _`Varnish documentation`: https://www.varnish-cache.org/docs/4.1/installation/install.html#compiling-varnish-from-source


Source code is built with autotools::

    sudo apt-get install libvarnishapi-dev || sudo yum install varnish-libs-devel
    ./bootstrap   # If running from git.
    ./configure
    make
    make check   # optional
    sudo make install


The resulting loadable modules (``libvmod_foo*.so`` files) will be installed to
the Varnish module directory. (default `/usr/lib/varnish/vmods/`)


Usage
-----

Each module has a different set of functions and usage, described in
separate documents in `docs/`. For completeness, here is a snippet from
`docs/cookie.rst`::

    import cookie;

    sub vcl_recv {
            cookie.parse(req.http.cookie);
            cookie.filter_except("SESSIONID,PHPSESSID");
            set req.http.cookie = cookie.get_string();
            # Only SESSIONID and PHPSESSID are left in req.http.cookie at this point.
    }



Development
-----------

The source git tree lives on Github: https://github.com/nigoroll/varnish-modules

Administrativa
--------------

The goals of this collection are:

* Simplify access to vmod code for Varnish users. One package to install, not 6.
* Decrease the maintenance cost that comes with having 10 different git
  repositories, each with autotools and (previously) distribution packaging files.

Expressed non-goals are:

* Import vmods that require external libraries, like curl or geoip. This
  collection should be simple and maintenance free to run.
* Support older releases of Varnish Cache.
* Include every vmod under the sun. We'll add the important ones.

Addition of further vmods is decided on a case-by-case basis. Code quality and
maintenance requirements will be important in this decision.


Contact
-------

This code is maintained by UPLEX based on the work of Varnish Software
- see https://github.com/varnish/varnish-modules

Issues can be reported via the Github issue tracker.

Other inquires can be sent to varnish dash support @__not_for_robots___uplex.de
