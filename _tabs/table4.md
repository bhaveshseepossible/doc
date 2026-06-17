---
title: Simple DataTables Demo
icon: fas fa-list
order: 8
script: true
---

<table id="demoTable"></table>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/simple-datatables@9.0.3/dist/style.css">
<script src="https://cdn.jsdelivr.net/npm/simple-datatables@9.0.3"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRw0zt06x2G1THOeFCTXeu_iHjKDTgXM44DbZpVemYW5-epnB_Q8FB2bgkcoMdna6LC3TKFnlfuoJNW/pub?output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        const table = document.getElementById("demoTable");
        const headers = Object.keys(res.data[0]);

        let html = "<thead><tr>";
        headers.forEach(function(col) {
          html += "<th>" + col + "</th>";
        });
        html += "</tr></thead><tbody>";

        res.data.forEach(function(row) {
          html += "<tr>";
          headers.forEach(function(headerName) {
            let val = row[headerName] || "";
            
            if (headerName.toLowerCase() === "photo" || headerName.toLowerCase() === "image") {
              html += '<td><img src="' + val + '" style="height:50px; width:50px; border-radius:4px;" /></td>';
            } else {
              html += "<td>" + val + "</td>";
            }
          });
          html += "</tr>";
        });
        html += "</tbody>";

        table.innerHTML = html;
        new simpleDatatables.DataTable(table);
      }
    }
  });
});
</script>
