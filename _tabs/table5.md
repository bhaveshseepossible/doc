---
title: AG Grid Demo
icon: fas fa-matrix
order: 9
script: true
---

<div id="myGrid" class="ag-theme-alpine" style="height: 600px; width:100%; margin: 20px 0;"></div>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ag-grid-community@31.0.0/styles/ag-grid.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ag-grid-community@31.0.0/styles/ag-theme-alpine.css">
<script src="https://cdn.jsdelivr.net/npm/ag-grid-community@31.0.0/dist/ag-grid-community.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRw0zt06x2G1THOeFCTXeu_iHjKDTgXM44DbZpVemYW5-epnB_Q8FB2bgkcoMdna6LC3TKFnlfuoJNW/pub?output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        ી
        const columnDefs = Object.keys(res.data[0]).map(function(key) {
          let colConfig = {
            headerName: key,
            field: key,
            sortable: true,
            filter: true,
            resizable: true
          };

          if (key.toLowerCase() === "photo" || key.toLowerCase() === "image") {
            colConfig.filter = false;
            colConfig.cellRenderer = function(params) {
              if (params.value) {
                return '<img src="' + params.value + '" style="height:45px; width:45px; border-radius:4px; object-fit:cover; margin-top:2px;"/>';
              }
              return '';
            };
          }

          return colConfig;
        });

        const gridOptions = {
          columnDefs: columnDefs,
          rowData: res.data,
          pagination: true,
          paginationPageSize: 20,
          paginationPageSizeSelector: [10, 20, 50, 100],
          rowHeight: 50
        };

        const gridDiv = document.querySelector('#myGrid');
        agGrid.createGrid(gridDiv, gridOptions);
      }
    }
  });
});
</script>
