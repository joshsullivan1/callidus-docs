Features
*********

Geography Hierarchy
====================

To distinguish parent and child Geographies, we have included various colors down to 7 “levels”.

.. image:: ../images/geography-hierarchy.png

Select Geographic Features
==========================

You can select multiple geographic features, including:

- Road
- Transit
- Landscape
- Water
- Points of interest

.. image:: ../images/select-geo-features.png

Heat Map of Accounts
====================

By toggling the button shown in the ``showMapViewerWidget()``, you can view the Accounts assoicated with a Geography based on the longitude and latitude of the Account within the area of the Polygon.

.. image:: ../images/heatmap-accounts.png

Draw Lasso of Accounts
=======================

Using the ``showMapViewerWidget()``, you can use one of the drawing tools to draw a Polygon and return a list of Account shown on the heatmap.

.. image:: ../images/draw-lasso.png

Data on Hover
=============

By hovering over a **Geography**, we currently show the number of **Accounts**.

.. image:: ../images/data-hover.png

External Data Source Assignment
================================

By click on the “Upload KML/KMZ” button, you can upload a file to assign a Polygon to a Geography.

.. image:: ../images/upload-file.png

API for Geolocation Information
===============================

We have provided a simple Javascript call that return the geocoded data for a particular location.

.. code-block:: Javascript

    window.navagis.geocodeAddress("nevada")
    .then(function (response) {
        // do stuff
    })

Sample Result

.. code-block:: Javascript

    "data": {
        "results": [
            {
                "address_components": [
                    {
                        "long_name": "Nevada",
                        "short_name": "NV",
                        "types": [
                            "administrative_area_level_1",
                            "political"
                        ]
                    },
                    {
                        "long_name": "United States",
                        "short_name": "US",
                        "types": [
                            "country",
                            "political"
                        ]
                    }
                ],
                "formatted_address": "Nevada, USA",
                "geometry": {
                    "bounds": {
                        "northeast": {
                            "lat": 42.002207,
                            "lng": -114.0396479
                        },
                        "southwest": {
                            "lat": 35.001857,
                            "lng": -120.0064729
                        }
                    },
                    "location": {
                        "lat": 38.8026097,
                        "lng": -116.419389
                    },
                    "location_type": "APPROXIMATE",
                    "viewport": {
                        "northeast": {
                            "lat": 42.002207,
                            "lng": -114.0396479
                        },
                        "southwest": {
                            "lat": 35.001857,
                            "lng": -120.0064729
                        }
                    }
                },
                "place_id": "ChIJcbTe-KEKmYARs5X8qooDR88",
                "types": [
                    "administrative_area_level_1",
                    "political"
                ]
            }
        ],
        "status": "OK"
    }

Refresh Geolocation Information
===============================

Endpoint: ``http://ip_address/v1/accounts/address/geocode/refresh``

The required parameter is Acocunt ID. You can pass in a list of Account IDs.

Example:

.. code-block:: Javascript

    window.navagis.refreshGeolocation(['509751182822998017', '509751182822998018', '509751182822998019'])
    .then(function (response) {
        // do stuff
    });

Response:

.. code-block:: Javascript

    "data": {
    "account_updated": [
        "509751182822998017",
        "509751182822998018"
        ]
    }

    // note only two accounts were updated cause the third account haven't reached the 30-day limit yet

Manual Syncing of Accounts
===========================

Endpoint: ``http://ip_address/v1/accounts/address/sync``

Example:

.. code-block:: Javascript

    window.navagis.syncAccounts(['509751182822998017', '509751182822998018', '509751182822998010'])
    .then(function (response) {
        // do stuff
    });

Response

.. code-block:: Javascript

    "data": {
        "syncedAccounts": [
            "509751182822998017",
            "509751182822998018"
        ],
        "notFoundAccounts":[
            "509751182822998010"
        ]
    }

Show Parent Geographies
========================

To show multiple parent Geographies on the same map, you can use the following function and pass in the ``geographySeq``:

``window.navagis.showGeographiesMap(['514536257427079169', '514536257427123269', '514536257427123169']);``

.. image:: ../images/show-parent-geos.png

Given Address within a Geography
=================================

.. code-block:: Javascript

    window.navagis.setAddressLookupCallback(function (data) {
        // do stuff 
    });

Data Grid Controls
===================

This shows the datagrid control map. The parameters are filter and callback. Callback will be invoked everytime the filter or the map bounds is changed and data contains all the accounts and geographies inside the map bounds.

.. code-block:: Javascript

    window.navagis.showDataGridControl({
        showAssignedAccounts: true,
        showUnassignedAccounts: false,
        showAssignedGeographies: false,
        showUnAssignedGeographies: false
    }, function (data) {
        // do stuff
    });

    window.navagis.setDataGridControlFilters({
        showAssignedAccounts: true,
        showUnassignedAccounts: false,
        showAssignedGeographies: true,
        showUnAssignedGeographies: false
    });

Sample Result for Callback

.. code-block:: Javascript

    {
        geographies: [
            {
                account_seq: "509751182822998017",
                account_type: 1,
                lat: "38.4239752",
                lng: "-121.4146915",
                status: "ASSIGNED"
            }
        ],
        accounts: [
            {
                id: "514536257427079169",
                name: "CA",
                org_id: null,
                parent_id: null,
                paths: [...],
                status: "ASSIGNED"
            }
        ]
    }

Show Territory Map
===================

To show a Territory map, use the following function:

``showTerritoryMap(tenantId, territorySeq, effStartDate, effEndDate)`` 

Example:

``window.navagis.showTerritoryMap('333', '507499383009323574', '2018-01-01', '2018-12-31');``

.. image:: ../images/show-territory.png

.. image:: ../images/show-territory-1.png


