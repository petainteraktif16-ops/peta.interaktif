[code (1).html](https://github.com/user-attachments/files/23940767/code.1.html)
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Peta Interaktif Pulau Sumatra</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background: linear-gradient(to bottom, #87CEEB 0%, #228B22 50%, #006400 100%);
            background-attachment: fixed;
            color: #333;
        }
        .container {
            display: flex;
            flex-wrap: wrap; /* Responsif untuk mobile */
            gap: 20px;
        }
        #map {
            flex: 1;
            min-width: 400px;
            height: 600px;
            border: 2px solid #000;
            background: rgba(255, 255, 255, 0.8);
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }
        #info {
            flex: 1;
            min-width: 300px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.9);
            border: 1px solid #ddd;
            border-radius: 5px;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
            height: fit-content; /* Sesuai konten */
        }
        h1, h2 {
            color: #fff;
            text-shadow: 1px 1px 2px #000;
        }
        p {
            margin: 0;
        }
    </style>
</head>
<body>
    <h1>Peta Interaktif Pulau Sumatra</h1>
    <p>Klik pada marker provinsi untuk melihat informasi detail (data diperbarui proyeksi 2025).</p>
    <div class="container">
        <div id="map"></div>
        <div id="info">
            <h2>Informasi Provinsi</h2>
            <p id="details">Klik marker untuk detail.</p>
        </div>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        // Inisialisasi peta dengan Leaflet.js, fokus pada Sumatra
        const map = L.map('map').setView([0.5, 102.0], 6); // Koordinat pusat Sumatra

        // Tambahkan tile layer (peta dasar dari OpenStreetMap)
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

        // Data provinsi Sumatra dengan koordinat perkiraan (data populasi diperbarui proyeksi 2025 berdasarkan pertumbuhan ~1.2% per tahun dari data BPS 2023)
        const provinces = {
            aceh: {
                name: "Aceh",
                coords: [4.6951, 96.7494],
                religion: "Islam (98%)",
                population: "5,5 juta (proyeksi 2025)",
                culture: "Budaya Aceh dengan adat istiadat seperti Meusekat, seni tari Saman, dan festival budaya.",
                language: "Bahasa Aceh, Bahasa Indonesia"
            },
            sumut: {
                name: "Sumatera Utara",
                coords: [2.1154, 99.5451],
                religion: "Islam (60%), Kristen (30%), Hindu/Buddha (10%)",
                population: "15,6 juta (proyeksi 2025)",
                culture: "Budaya Batak dengan upacara adat seperti Mangongkal Holi, musik gondang, dan rumah adat Bolon.",
                language: "Bahasa Batak (Toba, Karo, dll.), Bahasa Indonesia"
            },
            sumbar: {
                name: "Sumatera Barat",
                coords: [-0.7399, 100.8000],
                religion: "Islam (98%)",
                population: "5,8 juta (proyeksi 2025)",
                culture: "Budaya Minangkabau dengan matrilineal, rumah gadang, tari Piring, dan festival Tabuik.",
                language: "Bahasa Minangkabau, Bahasa Indonesia"
            },
            riau: {
                name: "Riau",
                coords: [0.2933, 101.7068],
                religion: "Islam (85%), Kristen/Buddha (15%)",
                population: "7,1 juta (proyeksi 2025)",
                culture: "Budaya Melayu dengan adat istiadat seperti kenduri, seni silat, dan rumah adat Limas.",
                language: "Bahasa Melayu Riau, Bahasa Indonesia"
            },
            jambi: {
                name: "Jambi",
                coords: [-1.4852, 102.4381],
                religion: "Islam (90%), Kristen/Buddha (10%)",
                population: "3,7 juta (proyeksi 2025)",
                culture: "Budaya Melayu Jambi dengan upacara adat seperti Sekapur Sirih, seni tari Zapin, dan rumah adat.",
                language: "Bahasa Jambi, Bahasa Indonesia"
            },
            sumsel: {
                name: "Sumatera Selatan",
                coords: [-3.3194, 104.1694],
                religion: "Islam (80%), Kristen/Buddha (20%)",
                population: "8,9 juta (proyeksi 2025)",
                culture: "Budaya Palembang dengan seni tari Tanggai, musik Gambus, dan festival Sriwijaya.",
                language: "Bahasa Palembang, Bahasa Indonesia"
            },
            bengkulu: {
                name: "Bengkulu",
                coords: [-3.5778, 102.3464],
                religion: "Islam (90%), Kristen/Buddha (10%)",
                population: "2,1 juta (proyeksi 2025)",
                culture: "Budaya Rejang dengan adat istiadat seperti Ngaben, seni tari Serampang, dan rumah adat.",
                language: "Bahasa Rejang, Bahasa Indonesia"
            },
            lampung: {
                name: "Lampung",
                coords: [-4.5586, 105.4068],
                religion: "Islam (85%), Kristen/Buddha (15%)",
                population: "9,3 juta (proyeksi 2025)",
                culture: "Budaya Lampung dengan adat Pepadun, seni tari Cuk, dan rumah adat Nuwo Sesat.",
                language: "Bahasa Lampung, Bahasa Indonesia"
            },
            kepri: {
                name: "Kepulauan Riau",
                coords: [0.1548, 104.5800],
                religion: "Islam (70%), Buddha (20%), Kristen (10%)",
                population: "2,3 juta (proyeksi 2025)",
                culture: "Budaya Melayu Kepri dengan seni musik Dangdut, adat istiadat laut, dan festival budaya.",
                language: "Bahasa Melayu Kepri, Bahasa Indonesia"
            },
            babel: {
                name: "Kepulauan Bangka Belitung",
                coords: [-2.7411, 106.4406],
                religion: "Islam (80%), Buddha (15%), Kristen (5%)",
                population: "1,6 juta (proyeksi 2025)",
                culture: "Budaya Melayu Bangka dengan adat istiadat seperti Kenduri, seni tari Ronggeng, dan rumah adat.",
                language: "Bahasa Bangka, Bahasa Indonesia"
            }
        };

        // Tambahkan marker untuk setiap provinsi
        Object.keys(provinces).forEach(key => {
            const prov = provinces[key];
            const marker = L.marker(prov.coords).addTo(map);
            marker.bindPopup(`<b>${prov.name}</b>`);
            marker.on('click', function() {
                document.getElementById('details').innerHTML = `
                    <strong>${prov.name}</strong><br>
                    Agama Mayoritas: ${prov.religion}<br>
                    Total Penduduk: ${prov.population}<br>
                    Budaya: ${prov.culture}<br>
                    Bahasa: ${prov.language}
                `;
            });
        });
    </script>
</body>
</html>
