---
title: GridJS Demo
icon: fas fa-th-large
order: 7
script: true
---

<div id="grid"></div>

<link href="https://unpkg.com/gridjs/dist/theme/mermaid.min.css" rel="stylesheet" />
<script src="https://unpkg.com/gridjs/dist/gridjs.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRw0zt06x2G1THOeFCTXeu_iHjKDTgXM44DbZpVemYW5-epnB_Q8FB2bgkcoMdna6LC3TKFnlfuoJNW/pub?output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        
        const headers = Object.keys(res.data[0]);
        
        const columnsConfig = headers.map(function(col) {
          if (col.toLowerCase() === "photo" || col.toLowerCase() === "image") {
            return {
              name: col,
              formatter: function(cell) {
                return gridjs.html('<img src="' + cell + '" style="height:50px; width:50px; border-radius:4px;"/>');
              }
            };
          }
          return col;
        });

        // ડેટા મેપિંગ (Arrow function વગર)
        const formattedData = res.data.map(function(row) {
          return Object.values(row);
        });

        new gridjs.Grid({
          columns: columnsConfig,
          data: formattedData,
          search: true,
          sort: true,
          pagination: true
        }).render(document.getElementById("grid"));
      }
    }
  });
});
</script>
