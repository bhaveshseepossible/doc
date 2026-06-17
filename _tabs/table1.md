---
# the default layout is 'page'
icon: fas fa-info-circle
order: 5
script: true
---

<div id="table" style="margin: 20px 0;"></div>

<link href="https://unpkg.com/tabulator-tables@6.3.0/dist/css/tabulator.min.css" rel="stylesheet">
<script src="https://unpkg.com/tabulator-tables@6.3.0/dist/js/tabulator.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRw0zt06x2G1THOeFCTXeu_iHjKDTgXM44DbZpVemYW5-epnB_Q8FB2bgkcoMdna6LC3TKFnlfuoJNW/pub?output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        
        const columns = Object.keys(res.data[0]).map(key => {
          
          let colConfig = {
            title: key,
            field: key,
            headerFilter: true
          };

          if (key.toLowerCase() === "photo" || key.toLowerCase() === "image") {
            colConfig.formatter = "image"; 
            colConfig.headerFilter = false; 
            
            colConfig.formatterParams = {
              height: "50px", 
              width: "50px", 
            };
          }

          return colConfig;
        });

        new Tabulator("#table", {
          data: res.data,
          columns: columns,
          layout: "fitColumns",
          pagination: true,
          paginationSize: 20,
          rowHeight: 60 
        });
      }
    }
  });
});
</script>
