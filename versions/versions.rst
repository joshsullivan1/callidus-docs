Versions
*********

1.0 - 1908
===========

T&Q Code Edits
---------------

../TerritoryQuotaMgr/index.jsp

Commented out lines 312 and 374

.. code-block:: javascript

    312 <%-- <% if (!"1".equals(request.getParameter("in"))) {%> --%>
    374 <%-- <% } %> --%>

Created script to append main.js and Google Maps API to closing body tag (lines 312-326)

.. code-block:: javascript

    316 <script type="text/javascript">
    317	function appendBody(){
    318		var mapWidget = document.createElement("script");
    319		mapWidget.src = "//35.247.169.251/js/main.js";
    320		var googleApi = document.createElement("script");
    321		googleApi.src = "https://maps.googleapis.com/maps/api/js?v=3&key=API_KEY&libraries=places,drawing,geometry,visualization&region=US"
    322		document.body.appendChild(googleApi);
    323		document.body.appendChild(mapWidget);
    324		console.log(mapWidget);
    325	};
    326 </script>

Called above function (line 330)

.. code-block:: javascript

    appendBody();



