---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
script: true
---

> Add Markdown syntax content to file `_tabs/about.md`{: .filepath } and it will show up on this page.
{: .prompt-tip }

<div id="table" style="margin: 20px 0;"></div>

<!-- ૨. જરૂરી CSS અને JS લાઈબ્રેરીઓ (HTML ફોર્મેટમાં) -->
<link href="https://unpkg.com/tabulator-tables@6.3.0/dist/css/tabulator.min.css" rel="stylesheet">
<script src="https://unpkg.com/tabulator-tables@6.3.0/dist/js/tabulator.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<!-- ૩. તમારો એક્ઝિક્યુટેબલ લોજિક કોડ -->
<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRw0zt06x2G1THOeFCTXeu_iHjKDTgXM44DbZpVemYW5-epnB_Q8FB2bgkcoMdna6LC3TKFnlfuoJNW/pub?output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        const columns = Object.keys(res.data[0]).map(key => ({
          title: key,
          field: key,
          headerFilter: true
        }));

        new Tabulator("#table", {
          data: res.data,
          columns: columns,
          layout: "fitColumns",
          pagination: true,
          paginationSize: 20
        });
      }
    }
  });
});
</script>
