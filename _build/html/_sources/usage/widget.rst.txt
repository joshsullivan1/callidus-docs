Map Widget API
**************

The following sections show you how to include the mapping widget and its functionality.

==============
Show Drawing Widget
==============

*Shows the drawing tools and upload file tool on the map.*

.. code-block:: javascript

    <script type="text/javascript">
      window.navagis.showDrawingWidget()
    </script>

.. image:: ../images/show-drawing-widget.png

==============
Show Geography
==============

*Shows a geography on the map.*

.. code-block:: javascript

    <script type="text/javascript">
      window.navagis.showGeographyMap(id, isEditable, showHierarchy)
    </script>

**Parameters:**

* **id** (required): id of the geography to be shown
* **isEditable** (default is false): boolean flag to determine whether the geography shown should be editable
* **showHierarchy** (default is false): boolean flag to determine whether to include view hierarchy functionality

.. image:: ../images/show-geography.png

Editable Geography
==================

If ``isEditable`` is set to true, you can reposition each vertices to adjust the boundaries of the geography.

.. image:: ../images/edit-geography.png

Hierarchy Functionality 
=======================

If ``showHierarchy`` is set to true, double left click on a geography to view child geographies and double right click to view parent geography 

.. image:: ../images/show-child-geographies.png

Full Example
============

.. image:: ../images/save-geography-example.png

.. code-block:: javascript
   :emphasize-lines: 9

    function onClickSave(data) {
        // create geography on backend
        ApiService.createGeography(data, function (response) {
            var id = response.geographySeq; // geographySeq of the newly created Geography
            var name = data.name; // name of the Geography that will be created
            var parentId = data.geographySeq; // geographySeq of selected parent geography on the image above
        
            // saves the drawn polygon as geography on navagis database
            window.navagis.saveGeography(id, name, parentId)
                .then(function (saveGeoResponse) {
                    console.log('save geo success: ', saveGeoResponse.data.message);
                }, function (error) {
                    alert('Error: ' + error.data.message);
                }
            );
        });
    }

.. note::
    
    - ``onClickSave(data)`` is invoked when the user clicks the “Save” button on the image above.
    - ``window.navagis.saveGeography(id, name, parentId)`` saves the new Geography. If no ``parentId`` is returned, the Geography will be saved as the parent. If ``parentId`` is returned, a ``child`` geography will be created.
    - ``ApiService.createGeography(data, callbackFunction)`` should be replaced with Callidus’s implementation on how to create geography, and that implementation should have a callback which returns the ``geographySeq`` of the newly created geography.
