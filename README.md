<script type="text/javascript">
        var gk_isXlsx = false;
        var gk_xlsxFileLookup = {};
        var gk_fileData = {};
        function filledCell(cell) {
          return cell !== '' && cell != null;
        }
        function loadFileData(filename) {
        if (gk_isXlsx && gk_xlsxFileLookup[filename]) {
            try {
                var workbook = XLSX.read(gk_fileData[filename], { type: 'base64' });
                var firstSheetName = workbook.SheetNames[0];
                var worksheet = workbook.Sheets[firstSheetName];

                // Convert sheet to JSON to filter blank rows
                var jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1, blankrows: false, defval: '' });
                // Filter out blank rows (rows where all cells are empty, null, or undefined)
                var filteredData = jsonData.filter(row => row.some(filledCell));

                // Heuristic to find the header row by ignoring rows with fewer filled cells than the next row
                var headerRowIndex = filteredData.findIndex((row, index) =>
                  row.filter(filledCell).length >= filteredData[index + 1]?.filter(filledCell).length
                );
                // Fallback
                if (headerRowIndex === -1 || headerRowIndex > 25) {
                  headerRowIndex = 0;
                }

                // Convert filtered JSON back to CSV
                var csv = XLSX.utils.aoa_to_sheet(filteredData.slice(headerRowIndex)); // Create a new sheet from filtered array of arrays
                csv = XLSX.utils.sheet_to_csv(csv, { header: 1 });
                return csv;
            } catch (e) {
                console.error(e);
                return "";
            }
        }
        return gk_fileData[filename] || "";
        }
</script>
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>מערכת ויזואליזציה - מגה ספורט</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #6e8efb, #a777e3);
            color: #fff;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 40px;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }

        .intro {
            max-width: 800px;
            text-align: center;
            margin-bottom: 40px;
            font-size: 1.2rem;
            line-height: 1.6;
        }

        .button-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: center;
        }

        .dashboard-button {
            background: linear-gradient(45deg, #ff6b6b, #ff8e53);
            border: none;
            padding: 15px 30px;
            font-size: 1.1rem;
            color: white;
            border-radius: 25px;
            cursor: pointer;
            transition: transform 0.3s, box-shadow 0.3s;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .dashboard-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        footer {
            margin-top: auto;
            padding: 20px;
            text-align: center;
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.7);
        }

        @media (max-width: 600px) {
            h1 {
                font-size: 1.8rem;
            }

            .dashboard-button {
                padding: 10px 20px;
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>המערכת של עדי - ויזואליזציה לקבלת החלטות במגה ספורט</h1>
    </header>

    <section class="intro">
        <p>המערכת הזו נבנתה כדי לעזור למגה ספורט לנתח נתוני מכירות, רווחיות, קטגוריות פופולריות והתנהגות לקוחות לאורך זמן. המטרה היא לאפשר קבלת החלטות טובה יותר בנוגע למלאי, אסטרטגיה ותפעול יומיומי.</p>
    </section>

    <section class="button-container">
        <button class="dashboard-button" onclick="window.location.href='dash1.html'">דאשבורד 1 - ניתוח ניהולי</button>
        <button class="dashboard-button" onclick="window.location.href='dash2.html'">דאשבורד 2 - ניתוח תפעולי</button>
        <button class="dashboard-button" onclick="window.location.href='dash3.html'">דאשבורד 3 - ניתוח אסטרטגי</button>
    </section>

    <footer>
        <p>© 2025 מגה ספורט - ויזואליזציה וקבלת החלטות | עדי</p>
    </footer>

    <script>
        document.querySelectorAll('.dashboard-button').forEach(button => {
            button.addEventListener('click', () => {
                button.style.transform = 'scale(0.95)';
                setTimeout(() => {
                    button.style.transform = 'scale(1)';
                }, 100);
            });
        });
    </script>
</body>
</html>
