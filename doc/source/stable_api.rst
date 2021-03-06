..
      Copyright 2015 Intel
      All Rights Reserved.

      Licensed under the Apache License, Version 2.0 (the "License"); you may
      not use this file except in compliance with the License. You may obtain
      a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

      Unless required by applicable law or agreed to in writing, software
      distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
      WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
      License for the specific language governing permissions and limitations
      under the License.


Nova Stable REST API
====================

This document describes both the current state of the Nova REST API -- as
of the Kilo release -- and also attempts to describe how the Nova team intends
to evolve the REST API's implementation over time and remove some of the
cruft that has crept in over the years.

Background
----------

Nova currently includes two distinct frameworks for exposing REST API
functionality. Older code is called the "V2 API" and exists in the
/nova/api/openstack/compute/contrib/ directory. Newer code is called the
"v2.1 API" and exists in the /nova/api/openstack/compute/plugins directory.

The V2 API is the old Nova REST API. It will be replaced by V2.1 API totally.
The code tree of V2 API will be removed in the future also.

The V2.1 API is the new Nova REST API with a set of improvements which
includes Mircoversion and standardized validation of inputs using JSON-Schema.
Also the V2.1 API is totally backwards compatible with the V2 API (That is the
reason we call it as V2.1 API).

Stable API
----------

In the V2 API, there is a concept called 'extension'. An operator can use it
to enable/disable part of Nova REST API based on requirements. And end user
may query the '/extensions' API to discover what *API functionality* is
supported by the Nova deployment.

Unfortunately, because V2 API extensions could be enabled or disabled
from one deployment to another -- as well as custom API extensions added
to one deployment and not another -- it was impossible for an end user to
know what the OpenStack Compute API actually included. No two OpenStack
deployments were consistent, which made cloud interoperability impossible.

API extensions, while not (yet) removed from the V2.1 API, are no longer
needed to evolve the REST API, and no new API functionality should use
the API extension classes to implement new functionality. Instead, new
API functionality should use the microversioning decorators to add or change
the REST API.

The extension is considered as two things in the Nova V2.1 API:

* The '/extensions' API

  In the V2 API the user can query it to determine what APIs are supported by
  the current Nova deployment.

  In V2.1 API, microversions enable us to add new features in backwards-
  compatible ways. And microversions not only enable us to add new futures by
  backwards-compatible method, also can be added by appropriate backwards-
  incompatible method.

  The '/extensions' API is frozen in Nova V2.1 API and will be deprecated
  in the future.

* The plugin framework

  One of the improvements in the V2.1 API was using stevedore to load
  Nova REST API extensions instead of old V2 handcrafted extension load
  mechanisim.

  There was an argument that the plugin framework supported extensibility in
  the Nova API to allow deployers to publish custom API resources.

  We will keep the existing plugin mechanisms in place within Nova but only
  to enable modularity in the codebase, not to allow extending of the Nova
  REST API.

As the extension will be removed from Nove V2.1 REST API. So the concept of
core API and extension API is eliminated also. There is no difference between
Nova V2.1 REST API, all of them are part of Nova stable REST API.
