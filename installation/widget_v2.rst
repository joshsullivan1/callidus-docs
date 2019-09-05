===================
Navagis Functions
===================

.. code-block:: javascript

    export default class Navagis {
        constructor(store) {
            this.store = store;
        }

        unmount(id = null) {
            let element = id
                          ? document.getElementById(id)
                          : document.getElementById('navagis-widget');

            unmountComponentAtNode(element);
        }

        redirect(url) {
            window.renderMyApp();
            history.replace('/');
            history.replace(url);
        }

        getMapOverlays() {
            let state = this.store.getState();

            if (state.hasOwnProperty('mapWidget') && state.mapWidget.hasOwnProperty('overlays')) {
                // todo transform data here to json
                return state.mapWidget.overlays;
            }

            return null;
        }

        showChildMapDrawingWidget(parentId, tenantId) {
            history.push(`/map-widget/draw/${parentId}/child?tenantId=${tenantId}`);
        }

        showMapDrawingWidget(tenantId, geographySeq=null, parentGeographySeq=null) {
            window.renderMyApp();
            history.replace('/');

            this.store.dispatch(showMapDrawingWidget(tenantId, geographySeq, parentGeographySeq));
        }

        showMapViewerWidget(tenantId) {
            history.push('/map-widget/view?tenantId=${tenantId}');
        }

        getGeographyParents() {
            return new Promise((resolve, reject) => {
                this.store.dispatch(getGeographyParents(resolve, reject));
            });
        }

        getGeographyChildren(id) {
            return new Promise((resolve, reject) => {
                this.store.dispatch(getGeographyChildren(id, resolve, reject));
            });
        }

        saveGeography(tenantId, geographySeq, parentGeographySeq=null) {
            return new Promise((resolve, reject) => {
                return this.store.dispatch(saveGeography(tenantId, geographySeq, parentGeographySeq, resolve, reject));
            });
        }

        showGeographyMap(id, tenantId) {
            this.redirect(`/map-widget/geography/${id}?tenantId=${tenantId}`);
        }

        showGeographiesMap(geographies) {
            GeographyService.setMultiDisplayGeographies(geographies);

            history.push('/map-widget/geographies');
        }

        showcreateChildGeography(id, name, parentId) {
            history.push(`/map-widget/geography/${parentId}/child/new?id=${id}&name=${name}`);
        }

        saveChildGeography(id, name, parentId) {
            return new Promise((resolve, reject) => {
                this.store.dispatch(saveGeography(id, name, parentId, resolve, reject));
            });
        }

        assignTerritoryGeography(territorySeq, geographySeq, tenantId = null) {
            return new Promise((resolve, reject) => {
                this.store.dispatch(assignTerritoryGeography(territorySeq, geographySeq, tenantId, resolve, reject));
            });
        }

        showTerritoryMap(tenantId, territoryProgramSeq, territorySeq=null, startDate=null, endDate=null, filters={accounts: {customer: {assigned: true, unassigned: false}, prospect: {assigned: true, unassigned: false} }, geographies: {assigned: true, unassigned: false}}, accountsUpdatedCallback=null, onMarkerClickedCallback=null) {
            window.renderMyApp();
            history.replace('/');

            if (!endDate) {
                let dateStr = '{m}/{d}/{y}',
                    today = new Date();

                endDate = dateStr.replace('{d}', String(today.getDate()).padStart(2, '0'))
                        .replace('{m}', String(today.getMonth() + 1).padStart(2, '0'))
                        .replace('{y}', today.getFullYear());
            }

            this.store.dispatch(setLoading(true));

            return new Promise((resolve, reject) => {
                this.store.dispatch(showTerritory(territoryProgramSeq, tenantId, territorySeq, startDate, endDate, filters, accountsUpdatedCallback, onMarkerClickedCallback, resolve, reject));
            }).then(function(){
                this.store.dispatch(setLoading(false));
            }.bind(this));
        }

        // territoryData = {territorySeq: '', name: '', effectiveStartDate: ''}
        onTerritoryClick(tenantId, territoryData) {
            this.store.dispatch(setLoading(true));

            return new Promise((resolve, reject) => {
                this.store.dispatch(onTerritoryClick(tenantId, territoryData, resolve, reject));
            }).then(function () {
                this.store.dispatch(setLoading(false));
            }.bind(this));
        }

        /**
         * id   = accountSeq or geographySeq
         * type = account or geography
         */
        onTerritoryMapItemClick(id, type, isSelect=true) {
            this.store.dispatch(onTerritoryMapItemClick(id, type, isSelect));
        }

        deselectTerritory(territorySeq) {
            this.store.dispatch(deselectTerritory(territorySeq));
        }

        updateTerritoryMapFilters(filters) {
            this.store.dispatch(setLoading(true));

            return new Promise((resolve, reject) => {
                this.store.dispatch(updateTerritoryMapFilters(filters, resolve, reject));
            }).then(function () {
                this.store.dispatch(setLoading(false));
            }.bind(this));
        }

        updateTerritoryMapData(data, filters={accounts: {customer: {assigned: true, unassigned: false}, prospect: {assigned: true, unassigned: false} }, geographies: {assigned: true, unassigned: false}}) {
            this.store.dispatch(updateTerritoryMapData(data, filters));
        }

        showTerritoryScenario(tenantId, srcTerritorySeq, srcStartDate, destTerritorySeq, destStartDate) {
            window.renderMyApp();
            history.replace('/');

            this.store.dispatch(setLoading(true));

            return new Promise((resolve, reject) => {
                this.store.dispatch(showTerritoryScenario(tenantId, srcTerritorySeq, srcStartDate, destTerritorySeq, destStartDate, resolve, reject));
            }).then(function(){
                this.store.dispatch(setLoading(false));
            }.bind(this));
        }

        geocodeAddress(address) {
            return ApiService.geocodeAddress(address);
        }

        refreshGeolocation(accountIds) {
            return ApiService.refreshGeolocation(accountIds);
        }

        syncAccounts(accountIds) {
            return ApiService.syncAccounts(accountIds);
        }

        showDataGridControl(filters = null, onDataChangedCallback) {
            history.push('/data-grid-control');
            DataGridControlService.setOnDataChangedCallback(onDataChangedCallback);
            if (filters) {
                this.setDataGridControlFilters(filters);
            }
        }

        setDataGridControlFilters(filters) {
            DataGridControlService.setFilters(filters);
        }

        setAddressLookupCallback(callback) {
            GeographyService.setAddressLookupCallback(callback);
        }
    }