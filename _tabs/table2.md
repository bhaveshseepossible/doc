---
title: DataTables Demo
icon: fas fa-table
order: 6
script: true
---

<table id="sheetTable" class="display" style="width:100%"></table>

<link rel="stylesheet" href="https://cdn.datatables.net/1.13.8/css/jquery.dataTables.min.css">
<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
<script src="https://cdn.datatables.net/1.13.8/js/jquery.dataTables.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
  Papa.parse(
    "https://docs.google.com/spreadsheets/d/e/2PACX-1vRw0zt06x2G1THOeFCTXeu_iHjKDTgXM44DbZpVemYW5-epnB_Q8FB2bgkcoMdna6LC3TKFnlfuoJNW/pub?output=csv",
    {
      download: true,
      header: true,
      complete: function(results) {
        if (results.data && results.data.length > 0) {
          
          const columns = Object.keys(results.data[0]).map(function(col) {
            let colConfig = {
              title: col,
              data: col
            };

            if (col.toLowerCase() === "photo" || col.toLowerCase() === "image") {
              colConfig.render = function(data) {
                return '<img src="' + data + '" style="height:50px; width:50px; object-fit:cover; border-radius:4px;"/>';
              };
            }
            return colConfig;
          });

          $('#sheetTable').DataTable({
            data: results.data,
            columns: columns,
            pageLength: 25,
            responsive: true
          });
        }
      }
    }
  );
});
</script>
