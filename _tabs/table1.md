---
# the default layout is 'page'
icon: fas fa-info-circle
order: 5
script: true
---


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
        
        // ડાયનેમિકલી કોલમ બનાવવાનો લોજિક
        const columns = Object.keys(res.data[0]).map(key => {
          
          // ડિફોલ્ટ સેટિંગ્સ બધી કોલમ માટે
          let colConfig = {
            title: key,
            field: key,
            headerFilter: true
          };

          // 🎯 જો કોલમનું નામ 'Photo' કે 'Image' હોય (તમારી શીટ મુજબ નામ બદલી નાખજો)
          if (key.toLowerCase() === "photo" || key.toLowerCase() === "image") {
            colConfig.formatter = "image"; // આ ઈમેજ ટેગ રેન્ડર કરશે
            colConfig.headerFilter = false; // ઈમેજ પર ફિલ્ટર બંધ કરવું સારું રહેશે
            
            // ઈમેજની સાઈઝ સેટ કરવા માટે કસ્ટમ પેરામીટર્સ (ઓપ્શનલ)
            colConfig.formatterParams = {
              height: "50px", // ઈમેજની હાઈટ 50 પિક્સલ
              width: "50px",  // ઈમેજની વિડ્થ 50 પિક્સલ
            };
          }

          return colConfig;
        });

        // Tabulator ટેબલ ક્રિએટ કરો
        new Tabulator("#table", {
          data: res.data,
          columns: columns,
          layout: "fitColumns",
          pagination: true,
          paginationSize: 20,
          rowHeight: 60 // ઈમેજ વ્યવસ્થિત દેખાય એટલે રો (Row) ની હાઈટ થોડી વધારી દીધી
        });
      }
    }
  });
});
</script>
