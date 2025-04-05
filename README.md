
<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لوحات الرمل التفاعلية</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
        }
        
        h1 {
            text-align: center;
            margin-bottom: 20px;
        }
        
        /* تنسيق مربع السؤال والتاريخ */
        .question-container {
            background-color: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }
        
        .question-input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
            margin-bottom: 10px;
        }
        
        .date-time-display {
            font-size: 16px;
            color: #666;
            text-align: left;
            margin-top: 10px;
        }
        
        .boards-container {
            display: flex;
            flex-wrap: nowrap;
            justify-content: center;
            gap: 15px;
            width: 100%;
            overflow-x: auto;
        }
        
        .board {
            background-color: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            flex: 1;
            min-width: 300px;
            max-width: 450px;
        }
        
        .board-title {
            text-align: center;
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 15px;
            color: #333;
        }
        
        .grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-template-rows: repeat(4, 1fr);
            gap: 10px;
        }
        
        .cell {
            border: 1px solid #ccc;
            aspect-ratio: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
        }
        
        /* تنسيق الأشكال حسب العناصر - الإطارات */
        /* الأشكال النارية (1-الجودلة، 2-الأحيان، 9-النصرة الخارجة، 15-الجماعة) */
        .fire-element {
            border: 2px solid #FF0000;
        }
        
        /* الأشكال الهوائية (3-العتبة الداخلة، 7-الحمرة، 11-الاجتماع، 16-القبض الداخل) */
        .air-element {
            border: 2px solid #A9A9A9; /* فضي غامق */
        }
        
        /* الأشكال المائية (4-البياض، 6-العتبة الخارجة، 13-الطريق، 14-القبض الخارج) */
        .water-element {
            border: 2px solid #0000FF;
        }
        
        /* الأشكال الترابية (5-نقي الخد، 8-الانكيس، 10-العقلة، 12-النصرة الداخلة) */
        .earth-element {
            border: 2px solid #8B4513; /* بني */
        }
        
        /* تنسيق لون النقاط حسب العناصر */
        .fire-dot {
            background-color: #FF0000 !important; /* أحمر */
        }
        
        .air-dot {
            background-color: #A9A9A9 !important; /* فضي غامق */
        }
        
        .water-dot {
            background-color: #0000FF !important; /* أزرق */
        }
        
        .earth-dot {
            background-color: #8B4513 !important; /* بني */
        }
        
        .cell-number {
            position: absolute;
            top: 2px;
            right: 2px;
            font-size: 12px;
            color: #666;
        }
        
        .figure-container {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 70%;
            width: 70%;
            margin: auto;
        }
        
        .dot-row {
            display: flex;
            justify-content: center;
            margin: 2px 0;
        }
        
        .dot {
            width: 5px;
            height: 5px;
            background-color: black;
            border-radius: 50%;
            margin: 0 2px;
        }
        
        select {
            margin-top: 5px;
            padding: 2px;
            font-size: 12px;
            width: 80%;
            text-align: center;
        }
        
        /* زر توليد لوحة باطن الرمل */
        .generate-button {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        
        .generate-button:hover {
            background-color: #0056b3;
        }
        
        /* زر حفظ اللوحات كصورة */
        .save-button {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        
        .save-button:hover {
            background-color: #218838;
        }
        
        /* تنسيق جدول الإحصائيات */
        .statistics-container {
            margin-top: 30px;
            background-color: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            max-width: 800px; /* تقليل العرض الأقصى */
            margin-left: auto;
            margin-right: auto;
        }
        
        .statistics-title {
            text-align: center;
            font-size: 28px; /* تكبير حجم الخط */
            font-weight: bold;
            margin-bottom: 15px;
            color: #333;
        }
        
        .statistics-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px; /* تقليل المسافة بين الأقسام */
        }
        
        .statistics-section {
            border: 1px solid #ddd; /* تقليل سمك الحدود */
            border-radius: 5px;
            padding: 8px; /* تقليل التباعد الداخلي */
            background-color: #f9f9f9;
        }
        
        .statistics-section h3 {
            text-align: center;
            margin-top: 0;
            margin-bottom: 10px;
            font-size: 22px; /* تكبير حجم الخط ليكون مثل العناوين */
            color: #333;
            border-bottom: 1px solid #007bff;
            padding-bottom: 5px;
        }
        
        .statistics-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 6px; /* تقليل المسافة بين العناصر */
            font-size: 20px; /* تكبير حجم الخط ليكون مثل العناوين */
            border-bottom: 1px solid #eee;
            padding-bottom: 3px;
        }
        
        .statistics-item .label {
            font-weight: normal;
        }
        
        .statistics-item .value {
            font-weight: bold;
            color: #007bff;
            min-width: 25px; /* تقليل العرض الأدنى */
            text-align: center;
            font-size: 22px; /* تكبير حجم الخط ليكون مثل العناوين */
        }
        
        .statistics-section:nth-child(3) .statistics-item .label,
        .statistics-section:nth-child(3) .statistics-item .value {
            font-size: 0.85em;
        }
        
        .copyright {
            text-align: center;
            margin-top: 30px;
            font-size: 14px;
            color: #666;
        }
        
        /* تحسين التجاوب مع الشاشات الصغيرة */
        @media (max-width: 1200px) {
            .statistics-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        
        @media (max-width: 768px) {
            .boards-container {
                flex-direction: column;
                align-items: center;
            }
            
            .board {
                min-width: 90%;
                max-width: 100%;
                margin-bottom: 20px;
            }
            
            .statistics-grid {
                grid-template-columns: 1fr;
            }
            
            .statistics-item {
                font-size: 18px; /* الحفاظ على حجم الخط في الشاشات الصغيرة */
            }
            
            .statistics-section h3 {
                font-size: 20px; /* الحفاظ على حجم الخط في الشاشات الصغيرة */
            }
            
            .statistics-title {
                font-size: 24px; /* تقليل حجم الخط قليلاً في الشاشات الصغيرة */
            }
            
            h1 {
                font-size: 20px;
            }
            
            .cell {
                min-height: 60px;
            }
        }
        
        @media (max-width: 480px) {
            body {
                padding: 10px;
            }
            
            .grid {
                gap: 5px;
            }
            
            .cell {
                min-height: 50px;
            }
            
            .dot {
                width: 4px;
                height: 4px;
            }
            
            .statistics-item {
                font-size: 16px; /* تقليل حجم الخط قليلاً في الشاشات الصغيرة جداً */
            }
        }

        /* إضافة تحسينات للتوافق مع الويب */
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            display: none;
        }

        .loading-spinner {
            border: 5px solid #f3f3f3;
            border-top: 5px solid #007bff;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* تحسين مظهر الأزرار */
        .generate-button, .save-button {
            transition: background-color 0.3s ease;
            font-weight: bold;
        }

        /* تحسين تجربة المستخدم عند التفاعل مع العناصر */
        .cell:hover {
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
        }

        select:hover {
            border-color: #007bff;
        }

        .question-input:focus {
            border-color: #007bff;
            box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
            outline: none;
        }
    </style>
</head>
<body>
    <h1>لوحات الرمل التفاعلية</h1>
    
    <!-- مربع السؤال والتاريخ -->
    <div class="question-container">
        <input type="text" id="question-input" class="question-input" placeholder="اكتب سؤالك أو المطلوب هنا...">
        <div class="date-time-display" id="date-time-display"></div>
    </div>
    
    <div class="boards-container">
        <div class="board" id="main-board">
            <div class="board-title">لوحة الرمل الرئيسية</div>
            <div class="grid"></div>
        </div>
        
        <div class="board" id="secondary-board">
            <div class="board-title">لوحة الرمل الفرعية</div>
            <div class="grid"></div>
        </div>
        
        <div class="board" id="inner-board">
            <div class="board-title">لوحة باطن الرمل</div>
            <div class="grid"></div>
        </div>
    </div>
    
    <button id="generate-inner-board" class="generate-button">توليد لوحة باطن الرمل</button>
    <button id="save-as-image" class="save-button" style="display: none;">حفظ اللوحات كصورة</button>
    
    <!-- جدول الإحصائيات -->
    <div class="statistics-container" id="statistics-container" style="display: none;">
        <div class="statistics-title">إحصائيات اللوحة الفرعية</div>
        <div class="statistics-grid">
            <!-- قسم إحصائيات مجموع النقاط -->
            <div class="statistics-section">
                <h3>إحصائيات مجموع النقاط</h3>
                <div class="statistics-item">
                    <span class="label">مجموع النقاط المفردة:</span>
                    <span class="value" id="total-single-dots">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">مجموع النقاط المزدوجة:</span>
                    <span class="value" id="total-double-dots">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">المجموع الكلي للنقاط:</span>
                    <span class="value" id="total-all-dots">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات العناصر -->
            <div class="statistics-section">
                <h3>إحصائيات نقاط العناصر</h3>
                <div class="statistics-item">
                    <span class="label">عدد نقاط النار المفردة:</span>
                    <span class="value" id="fire-points">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد نقاط الهواء المفردة:</span>
                    <span class="value" id="air-points">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد نقاط الماء المفردة:</span>
                    <span class="value" id="water-points">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد نقاط التراب المفردة:</span>
                    <span class="value" id="earth-points">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات أشكال العناصر -->
            <div class="statistics-section">
                <h3>إحصائيات أشكال العناصر</h3>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال النارية:</span>
                    <span class="value" id="fire-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الهوائية:</span>
                    <span class="value" id="air-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال المائية:</span>
                    <span class="value" id="water-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الترابية:</span>
                    <span class="value" id="earth-figures">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات أنواع الأشكال -->
            <div class="statistics-section">
                <h3>إحصائيات أنواع الأشكال</h3>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الخارجة:</span>
                    <span class="value" id="outer-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الداخلة:</span>
                    <span class="value" id="inner-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال المنقلبة:</span>
                    <span class="value" id="mobile-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الثابتة:</span>
                    <span class="value" id="stable-figures">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات السعد والنحس -->
            <div class="statistics-section">
                <h3>إحصائيات السعد والنحس</h3>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال السعيدة:</span>
                    <span class="value" id="fortunate-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال النحيسة:</span>
                    <span class="value" id="unfortunate-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الممتزجة سعد:</span>
                    <span class="value" id="mixed-fortunate-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الممتزجة نحس:</span>
                    <span class="value" id="mixed-unfortunate-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الممتزجة بين سعد ونحس:</span>
                    <span class="value" id="mixed-figures">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات الصدق والكذب -->
            <div class="statistics-section">
                <h3>إحصائيات الصدق والكذب</h3>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الصادقة:</span>
                    <span class="value" id="truthful-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الكاذبة:</span>
                    <span class="value" id="false-figures">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات الجنس -->
            <div class="statistics-section">
                <h3>إحصائيات الجنس</h3>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال المذكرة:</span>
                    <span class="value" id="masculine-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال المؤنثة:</span>
                    <span class="value" id="feminine-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد الأشكال الخنثى:</span>
                    <span class="value" id="neutral-figures">0</span>
                </div>
            </div>
            
            <!-- قسم إحصائيات الكواكب -->
            <div class="statistics-section">
                <h3>إحصائيات الكواكب</h3>
                <div class="statistics-item">
                    <span class="label">عدد أشكال الشمس:</span>
                    <span class="value" id="sun-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال القمر:</span>
                    <span class="value" id="moon-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال المريخ:</span>
                    <span class="value" id="mars-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال عطارد:</span>
                    <span class="value" id="mercury-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال المشتري:</span>
                    <span class="value" id="jupiter-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال الزهرة:</span>
                    <span class="value" id="venus-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال زحل:</span>
                    <span class="value" id="saturn-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال الرأس:</span>
                    <span class="value" id="head-figures">0</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عدد أشكال الذنب:</span>
                    <span class="value" id="tail-figures">0</span>
                </div>
            </div>
            
            <!-- قسم مفردات ومزدوجات الرمل (إضافة جديدة) -->
            <div class="statistics-section">
                <h3>مفردات ومزدوجات الرمل</h3>
                <div class="statistics-item">
                    <span class="label">مفردات الرمل للسؤال:</span>
                    <span class="value" id="sand-singles-name">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">مزدوجات الرمل للجواب:</span>
                    <span class="value" id="sand-doubles-name">-</span>
                </div>
            </div>
            
            <!-- قسم طرق حساب الضمير (إضافة جديدة) -->
            <div class="statistics-section">
                <h3>طرق حساب الضمير</h3>
                <div class="statistics-item">
                    <span class="label">مجموع نقاط الرمل / 12:</span>
                    <span class="value" id="dameer-method1">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">حساب ابدح للأمهات:</span>
                    <span class="value" id="dameer-method2">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عناصر الأمهات:</span>
                    <span class="value" id="dameer-method3">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">عنصر النار للأشكال:</span>
                    <span class="value" id="dameer-method4">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">نقاط النار والتراب (1-15):</span>
                    <span class="value" id="dameer-method5">-</span>
                </div>
            </div>
            
            <!-- إضافة قسم معلومات الطالع -->
            <div class="statistics-section">
                <h3>معلومات الطالع</h3>
                <div class="statistics-item">
                    <span class="label">شكل الطالع:</span>
                    <span class="value" id="ascendant-figure-name">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">كوكب الطالع:</span>
                    <span class="value" id="ascendant-planet">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">نوع الطالع:</span>
                    <span class="value" id="ascendant-type">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">سعد/نحس الطالع:</span>
                    <span class="value" id="ascendant-fortune">-</span>
                </div>
            </div>
            
            <!-- إضافة قسم حركة شكل الطالع -->
            <div class="statistics-section">
                <h3>حركة شكل الطالع</h3>
                <div class="statistics-item">
                    <span class="label">اسم الشكل:</span>
                    <span class="value" id="ascendant-movement-name">-</span>
                </div>
                <div class="statistics-item">
                    <span class="label">رقم الحركة من بيته:</span>
                    <span class="value" id="ascendant-movement-number">-</span>
                </div>
            </div>
        </div>
    </div>
    
    <div class="copyright">
        جميع الحقوق محفوظة &copy; 2025 | لوحات الرمل التفاعلية
    </div>
    
    <!-- إضافة عنصر التحميل -->
    <div class="loading-overlay" id="loading-overlay">
        <div class="loading-spinner"></div>
    </div>
    
    <!-- إضافة مكتبة html2canvas لتحويل اللوحات إلى صورة -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js" integrity="sha512-BNaRQnYJYiPSqHHDb58B0yaPfCu+Wgds8Gp/gU33kqBtgNS4tSPHuGibyoeqMV/TJlSKda6FXzoEyYGjTe+vXA==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    
    <script>
        // تعريف أشكال الرمل مع خصائصها
        const geomancyFigures = [
            { 
                pattern: [1, 1, 2, 1], 
                element: 'fire',
                type: 'mobile',
                fortune: 'mixed-fortunate',
                truth: 'truthful',
                gender: 'neutral-masculine',
                planet: 'mars'
            }, // 1 - الجودلة
            { 
                pattern: [1, 2, 2, 2], 
                element: 'fire',
                type: 'outer',
                fortune: 'fortunate',
                truth: 'truthful',
                gender: 'masculine',
                planet: 'jupiter'
            }, // 2 - الأحيان
            { 
                pattern: [2, 1, 1, 1], 
                element: 'air',
                type: 'inner',
                fortune: 'fortunate',
                truth: 'truthful',
                gender: 'feminine',
                planet: 'jupiter'
            }, // 3 - العتبة الداخلة
            { 
                pattern: [2, 2, 1, 2], 
                element: 'water',
                type: 'mobile',
                fortune: 'fortunate',
                truth: 'truthful',
                gender: 'feminine',
                planet: 'moon'
            }, // 4 - البياض
            { 
                pattern: [1, 2, 1, 1], 
                element: 'earth',
                type: 'mobile',
                fortune: 'mixed-unfortunate',
                truth: 'false',
                gender: 'neutral-feminine',
                planet: 'venus'
            }, // 5 - نقي الخد
            { 
                pattern: [1, 1, 1, 2], 
                element: 'water',
                type: 'outer',
                fortune: 'unfortunate',
                truth: 'false',
                gender: 'feminine',
                planet: 'tail'
            }, // 6 - العتبة الخارجة
            { 
                pattern: [2, 1, 2, 2], 
                element: 'air',
                type: 'mobile',
                fortune: 'unfortunate',
                truth: 'false',
                gender: 'masculine',
                planet: 'mars'
            }, // 7 - الحمرة
            { 
                pattern: [2, 2, 2, 1], 
                element: 'earth',
                type: 'inner',
                fortune: 'unfortunate',
                truth: 'false',
                gender: 'feminine',
                planet: 'saturn'
            }, // 8 - الانكيس
            { 
                pattern: [1, 1, 2, 2], 
                element: 'fire',
                type: 'outer',
                fortune: 'fortunate',
                truth: 'false',
                gender: 'masculine',
                planet: 'sun'
            }, // 9 - النصرة الخارجة
            { 
                pattern: [1, 2, 2, 1], 
                element: 'earth',
                type: 'stable',
                fortune: 'mixed-unfortunate',
                truth: 'false',
                gender: 'feminine',
                planet: 'saturn'
            }, // 10 - العقلة
            { 
                pattern: [2, 1, 1, 2], 
                element: 'air',
                type: 'stable',
                fortune: 'mixed-fortunate',
                truth: 'truthful',
                gender: 'masculine',
                planet: 'mercury'
            }, // 11 - الاجتماع
            { 
                pattern: [2, 2, 1, 1], 
                element: 'earth',
                type: 'inner',
                fortune: 'fortunate',
                truth: 'truthful',
                gender: 'feminine',
                planet: 'venus'
            }, // 12 - النصرة الداخلة
            { 
                pattern: [1, 1, 1, 1], 
                element: 'water',
                type: 'stable',
                fortune: 'mixed-fortunate',
                truth: 'truthful',
                gender: 'masculine',
                planet: 'moon'
            }, // 13 - الطريق
            { 
                pattern: [1, 2, 1, 2], 
                element: 'water',
                type: 'outer',
                fortune: 'unfortunate',
                truth: 'false',
                gender: 'masculine',
                planet: 'head'
            }, // 14 - القبض الخارج
            { 
                pattern: [2, 2, 2, 2], 
                element: 'fire',
                type: 'stable',
                fortune: 'mixed',
                truth: 'truthful',
                gender: 'masculine',
                planet: 'mercury'
            }, // 15 - الجماعة
            { 
                pattern: [2, 1, 2, 1], 
                element: 'air',
                type: 'inner',
                fortune: 'fortunate',
                truth: 'false',
                gender: 'feminine',
                planet: 'sun'
            }  // 16 - القبض الداخل
        ];
        
        // متغير لتخزين التاريخ والوقت عند اكتمال اللوحة الفرعية
        let capturedDateTime = null;
        
        // إضافة الأسماء العربية للأشكال
        function addArabicNamesToFigures() {
            // تعريف الأسماء العربية للأشكال وفقاً للترتيب التقليدي
            const arabicNames = [
                "الجودله", // PUER (1)
                "الاحيان", // LAETITIA (2)
                "عتبه داخل", // CAPUT DRACONIS (3)
                "بياض", // ALBUS (4)
                "نقي الخد", // PUELLA (5)
                "عتبه خارجه", // CAUDA DRACONIS (6)
                "الحمره", // RUBEUS (7)
                "انكيس", // TRISTITIA (8)
                "نصره خارجه", // FORTUNA MINOR (9)
                "عقله", // CARCER (10)
                "احتماع", // CONJUNCTIO (11)
                "نصره داخل", // FORTUNA MAJOR (12)
                "طريق", // VIA (13)
                "قبضه خارج", // AMISSIO (14)
                "جماعه", // POPULUS (15)
                "قبضه داخل" // ACQUISITIO (16)
            ];

            // إضافة الأسماء العربية إلى مصفوفة الأشكال
            for (let i = 0; i < geomancyFigures.length; i++) {
                geomancyFigures[i].arabicName = arabicNames[i];
            }
        }
        
        // إنشاء عنصر شكل الرمل
        function createFigureElement(pattern, figureIndex) {
            const figureContainer = document.createElement('div');
            figureContainer.className = 'figure-container';
            
            // إضافة خاصية data-figure-index لتتبع مؤشر الشكل
            if (figureIndex !== undefined) {
                figureContainer.dataset.figureIndex = figureIndex;
            }
            
            // تحديد عنصر الشكل لتلوين النقاط
            let elementClass = '';
            if (figureIndex !== undefined) {
                elementClass = geomancyFigures[figureIndex].element;
            } else {
                // البحث عن الشكل المطابق في مصفوفة الأشكال
                for (let i = 0; i < geomancyFigures.length; i++) {
                    if (patternsEqual(geomancyFigures[i].pattern, pattern)) {
                        elementClass = geomancyFigures[i].element;
                        break;
                    }
                }
            }
            
            // إنشاء صفوف النقاط
            for (let i = 0; i < pattern.length; i++) {
                const dotRow = document.createElement('div');
                dotRow.className = 'dot-row';
                
                // إضافة النقاط لكل صف
                for (let j = 0; j < pattern[i]; j++) {
                    const dot = document.createElement('div');
                    dot.className = 'dot';
                    
                    // إضافة فئة لون العنصر للنقطة
                    if (elementClass) {
                        dot.classList.add(`${elementClass}-dot`);
                    }
                    
                    dotRow.appendChild(dot);
                }
                
                figureContainer.appendChild(dotRow);
            }
            
            return figureContainer;
        }

        // تحديد فئة العنصر بناءً على نمط الشكل
        function getElementClassByPattern(pattern) {
            // البحث عن الشكل المطابق في مصفوفة الأشكال
            for (let i = 0; i < geomancyFigures.length; i++) {
                if (patternsEqual(geomancyFigures[i].pattern, pattern)) {
                    // إرجاع فئة العنصر المناسبة
                    return `${geomancyFigures[i].element}-element`;
                }
            }
            
            return '';
        }

        // إنشاء اللوحات الثلاث
        function createBoards() {
            const mainBoardGrid = document.querySelector('#main-board .grid');
            const secondaryBoardGrid = document.querySelector('#secondary-board .grid');
            const innerBoardGrid = document.querySelector('#inner-board .grid');
            
            // إنشاء خلايا اللوحات
            for (let i = 0; i < 16; i++) {
                // حساب رقم المربع (1-16)
                const cellNumber = i + 1;
                
                // تحديد فئة العنصر للشكل
                const elementClass = `${geomancyFigures[i].element}-element`;
                
                // إنشاء خلية للوحة الرئيسية
                const mainCell = document.createElement('div');
                mainCell.className = `cell ${elementClass}`;
                mainCell.id = `main-cell-${cellNumber}`;
                
                // إضافة رقم المربع
                const mainCellNumber = document.createElement('div');
                mainCellNumber.className = 'cell-number';
                mainCellNumber.textContent = cellNumber;
                mainCell.appendChild(mainCellNumber);
                
                // إضافة الشكل للوحة الرئيسية
                const figureElement = createFigureElement(geomancyFigures[i].pattern, i);
                mainCell.appendChild(figureElement);
                
                mainBoardGrid.appendChild(mainCell);
                
                // إنشاء خلية للوحة الفرعية
                const secondaryCell = document.createElement('div');
                secondaryCell.className = 'cell'; // سيتم تحديد الفئة لاحقاً عند اختيار الشكل
                secondaryCell.id = `secondary-cell-${cellNumber}`;
                
                // إضافة رقم المربع
                const secondaryCellNumber = document.createElement('div');
                secondaryCellNumber.className = 'cell-number';
                secondaryCellNumber.textContent = cellNumber;
                secondaryCell.appendChild(secondaryCellNumber);
                
                // إضافة قائمة منسدلة لمربعات الأمهات (1-4) في اللوحة الفرعية
                if (i < 4) {
                    const select = document.createElement('select');
                    select.id = `select-${cellNumber}`;
                    
                    // إضافة خيار فارغ
                    const emptyOption = document.createElement('option');
                    emptyOption.value = '';
                    emptyOption.textContent = 'اختر';
                    select.appendChild(emptyOption);
                    
                    // إضافة خيارات الأشكال بأسمائها
                    for (let figIndex = 0; figIndex < 16; figIndex++) {
                        const option = document.createElement('option');
                        option.value = figIndex;
                        option.textContent = geomancyFigures[figIndex].arabicName;
                        select.appendChild(option);
                    }
                    
                    // إضافة حدث تغيير القائمة المنسدلة
                    select.addEventListener('change', function() {
                        const figureIndex = this.value;
                        const figureContainer = secondaryCell.querySelector('.figure-container');
                        
                        // إزالة الشكل السابق إن وجد
                        if (figureContainer) {
                            figureContainer.remove();
                        }
                        
                        // إضافة الشكل الجديد إذا تم اختيار شكل
                        if (figureIndex !== '') {
                            const figure = geomancyFigures[figureIndex];
                            const newFigureElement = createFigureElement(figure.pattern, figureIndex);
                            secondaryCell.insertBefore(newFigureElement, select); // إضافة الشكل قبل القائمة المنسدلة
                            
                            // تحديث فئة العنصر للخلية
                            secondaryCell.className = `cell ${figure.element}-element`;
                            
                            // التحقق من اكتمال أشكال الأمهات وتوليد جميع الأشكال تلقائياً
                            setTimeout(generateAllFigures, 100); // تأخير قصير لضمان اكتمال التحديث
                        } else {
                            // إعادة تعيين فئة الخلية إلى الافتراضية
                            secondaryCell.className = 'cell';
                        }
                    });
                    
                    secondaryCell.appendChild(select);
                }
                
                secondaryBoardGrid.appendChild(secondaryCell);
                
                // إنشاء خلية للوحة باطن الرمل
                const innerCell = document.createElement('div');
                innerCell.className = 'cell'; // سيتم تحديد الفئة لاحقاً عند توليد الشكل
                innerCell.id = `inner-cell-${cellNumber}`;
                
                // إضافة رقم المربع
                const innerCellNumber = document.createElement('div');
                innerCellNumber.className = 'cell-number';
                innerCellNumber.textContent = cellNumber;
                innerCell.appendChild(innerCellNumber);
                
                innerBoardGrid.appendChild(innerCell);
            }
        }

        // التحقق من اختيار جميع أشكال الأمهات
        function checkAllMothersSelected() {
            for (let i = 1; i <= 4; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return false;
                }
            }
            
            return true;
        }

        // استخراج نمط النقاط من عنصر الشكل
        function extractPatternFromFigure(figureContainer) {
            const dotRows = figureContainer.querySelectorAll('.dot-row');
            let pattern = [];
            
            dotRows.forEach(row => {
                const dots = row.querySelectorAll('.dot');
                pattern.push(dots.length);
            });
            
            return pattern;
        }

        // عرض شكل في خلية محددة
        function displayFigure(cellNumber, pattern) {
            const cell = document.getElementById(`secondary-cell-${cellNumber}`);
            const oldFigure = cell.querySelector('.figure-container');
            
            if (oldFigure) {
                oldFigure.remove();
            }
            
            // البحث عن مؤشر الشكل المطابق للنمط
            const figureIndex = findFigureIndexByPattern(pattern);
            
            // إنشاء عنصر الشكل الجديد
            const figureElement = createFigureElement(pattern, figureIndex);
            cell.appendChild(figureElement);
            
            // تحديث فئة العنصر للخلية
            if (figureIndex !== -1) {
                cell.className = `cell ${geomancyFigures[figureIndex].element}-element`;
            } else {
                // إذا لم يتم العثور على الشكل، استخدم الفئة بناءً على النمط
                cell.className = `cell ${getElementClassByPattern(pattern)}`;
            }
        }

        // ضرب نقطتين معاً وفقاً لقواعد علم الرمل
        function multiplyDots(dots1, dots2) {
            // نقطة × نقطة = نقطتان
            if (dots1 === 1 && dots2 === 1) {
                return 2;
            }
            // نقطة × نقطتان = نقطة
            else if ((dots1 === 1 && dots2 === 2) || (dots1 === 2 && dots2 === 1)) {
                return 1;
            }
            // نقطتان × نقطتان = نقطتان
            else if (dots1 === 2 && dots2 === 2) {
                return 2;
            }
            
            // حالة غير متوقعة
            console.error("قيم غير صالحة للضرب:", dots1, dots2);
            return 1;
        }

        // ضرب شكلين معاً
        function multiplyFigures(figure1, figure2) {
            let resultPattern = [];
            
            for (let i = 0; i < 4; i++) {
                resultPattern.push(multiplyDots(figure1[i], figure2[i]));
            }
            
            return resultPattern;
        }

        // توليد أشكال البنات (5-8)
        function generateDaughterFigures() {
            // التحقق من وجود أشكال الأمهات
            for (let i = 1; i <= 4; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    console.error(`الشكل ${i} غير موجود`);
                    return;
                }
            }
            
            // استخراج أنماط الأشكال
            const figure1Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-1').querySelector('.figure-container'));
            const figure2Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-2').querySelector('.figure-container'));
            const figure3Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-3').querySelector('.figure-container'));
            const figure4Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-4').querySelector('.figure-container'));
            
            // توليد أشكال البنات وفقاً للقواعد
            // المربع 5 = الصف الأول من المربعات 1-4
            const figure5Pattern = [figure1Pattern[0], figure2Pattern[0], figure3Pattern[0], figure4Pattern[0]];
            
            // المربع 6 = الصف الثاني من المربعات 1-4
            const figure6Pattern = [figure1Pattern[1], figure2Pattern[1], figure3Pattern[1], figure4Pattern[1]];
            
            // المربع 7 = الصف الثالث من المربعات 1-4
            const figure7Pattern = [figure1Pattern[2], figure2Pattern[2], figure3Pattern[2], figure4Pattern[2]];
            
            // المربع 8 = الصف الرابع من المربعات 1-4
            const figure8Pattern = [figure1Pattern[3], figure2Pattern[3], figure3Pattern[3], figure4Pattern[3]];
            
            // عرض أشكال البنات في المربعات 5-8
            displayFigure(5, figure5Pattern);
            displayFigure(6, figure6Pattern);
            displayFigure(7, figure7Pattern);
            displayFigure(8, figure8Pattern);
        }

        // توليد أشكال الحفيدات (9-12)
        function generateGranddaughterFigures() {
            // التحقق من وجود أشكال البنات
            for (let i = 5; i <= 8; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    console.error(`الشكل ${i} غير موجود`);
                    return;
                }
            }
            
            // استخراج أنماط الأشكال
            const figure1Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-1').querySelector('.figure-container'));
            const figure2Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-2').querySelector('.figure-container'));
            const figure3Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-3').querySelector('.figure-container'));
            const figure4Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-4').querySelector('.figure-container'));
            const figure5Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-5').querySelector('.figure-container'));
            const figure6Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-6').querySelector('.figure-container'));
            const figure7Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-7').querySelector('.figure-container'));
            const figure8Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-8').querySelector('.figure-container'));
            
            // ضرب الأشكال وفقاً للقواعد
            // المربع 9 = ضرب المربع 1 مع المربع 2
            const figure9Pattern = multiplyFigures(figure1Pattern, figure2Pattern);
            
            // المربع 10 = ضرب المربع 3 مع المربع 4
            const figure10Pattern = multiplyFigures(figure3Pattern, figure4Pattern);
            
            // المربع 11 = ضرب المربع 5 مع المربع 6
            const figure11Pattern = multiplyFigures(figure5Pattern, figure6Pattern);
            
            // المربع 12 = ضرب المربع 7 مع المربع 8
            const figure12Pattern = multiplyFigures(figure7Pattern, figure8Pattern);
            
            // عرض أشكال الحفيدات في المربعات 9-12
            displayFigure(9, figure9Pattern);
            displayFigure(10, figure10Pattern);
            displayFigure(11, figure11Pattern);
            displayFigure(12, figure12Pattern);
        }

        // توليد أشكال الزوائد (13-16)
        function generateExtraFigures() {
            // التحقق من وجود أشكال الحفيدات
            for (let i = 9; i <= 12; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    console.error(`الشكل ${i} غير موجود`);
                    return;
                }
            }
            
            // استخراج أنماط الأشكال
            const figure1Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-1').querySelector('.figure-container'));
            const figure9Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-9').querySelector('.figure-container'));
            const figure10Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-10').querySelector('.figure-container'));
            const figure11Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-11').querySelector('.figure-container'));
            const figure12Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-12').querySelector('.figure-container'));
            
            // ضرب الأشكال وفقاً للقواعد
            // المربع 13 = ضرب المربع 9 مع المربع 10
            const figure13Pattern = multiplyFigures(figure9Pattern, figure10Pattern);
            
            // المربع 14 = ضرب المربع 11 مع المربع 12
            const figure14Pattern = multiplyFigures(figure11Pattern, figure12Pattern);
            
            // عرض أشكال الزوائد في المربعات 13-14
            displayFigure(13, figure13Pattern);
            displayFigure(14, figure14Pattern);
            
            // استخراج أنماط الأشكال الجديدة
            setTimeout(() => {
                const figure13Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-13').querySelector('.figure-container'));
                const figure14Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-14').querySelector('.figure-container'));
                
                // المربع 15 = ضرب المربع 13 مع المربع 14
                const figure15Pattern = multiplyFigures(figure13Pattern, figure14Pattern);
                
                // عرض الشكل 15
                displayFigure(15, figure15Pattern);
                
                // استخراج نمط الشكل 15 الجديد
                setTimeout(() => {
                    const figure15Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-15').querySelector('.figure-container'));
                    
                    // المربع 16 = ضرب المربع 15 مع المربع 1
                    const figure16Pattern = multiplyFigures(figure15Pattern, figure1Pattern);
                    
                    // عرض الشكل 16
                    displayFigure(16, figure16Pattern);
                    
                    // تحديث الإحصائيات بعد اكتمال جميع الأشكال
                    setTimeout(() => {
                        updateStatistics();
                        
                        // إظهار زر حفظ اللوحات كصورة
                        document.getElementById('save-as-image').style.display = 'block';
                    }, 100);
                }, 100);
            }, 100);
        }

        // توليد جميع الأشكال
        function generateAllFigures() {
            // التحقق من وجود أشكال في مربعات الأمهات
            if (!checkAllMothersSelected()) {
                return; // لم تكتمل أشكال الأمهات بعد
            }
            
            // تسجيل التاريخ والوقت عند اكتمال أشكال الأمهات إذا لم يتم تسجيله من قبل
            if (!capturedDateTime) {
                capturedDateTime = getCurrentDateTime();
                document.getElementById('date-time-display').textContent = capturedDateTime;
            }
            
            // 1. توليد أشكال البنات (5-8)
            generateDaughterFigures();
            
            // التأكد من وجود أشكال البنات قبل المتابعة
            setTimeout(() => {
                // 2. توليد أشكال الحفيدات (9-12)
                generateGranddaughterFigures();
                
                // التأكد من وجود أشكال الحفيدات قبل المتابعة
                setTimeout(() => {
                    // 3. توليد أشكال الزوائد (13-16)
                    generateExtraFigures();
                }, 100);
            }, 100);
        }

        // دالة للبحث عن مؤشر الشكل بناءً على النمط
        function findFigureIndexByPattern(pattern) {
            for (let i = 0; i < geomancyFigures.length; i++) {
                const figPattern = geomancyFigures[i].pattern;
                if (patternsEqual(figPattern, pattern)) {
                    return i;
                }
            }
            return -1;
        }

        // دالة للتحقق من تطابق نمطين
        function patternsEqual(pattern1, pattern2) {
            if (pattern1.length !== pattern2.length) {
                return false;
            }
            
            for (let i = 0; i < pattern1.length; i++) {
                if (pattern1[i] !== pattern2[i]) {
                    return false;
                }
            }
            
            return true;
        }

        // توليد لوحة باطن الرمل
        function generateInnerBoard() {
            // التحقق من اكتمال اللوحة الفرعية
            for (let i = 1; i <= 16; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    alert('يجب اكتمال جميع أشكال اللوحة الفرعية أولاً');
                    return;
                }
            }
            
            // توليد أشكال لوحة باطن الرمل
            for (let i = 1; i <= 16; i++) {
                const mainCell = document.getElementById(`main-cell-${i}`);
                const secondaryCell = document.getElementById(`secondary-cell-${i}`);
                const innerCell = document.getElementById(`inner-cell-${i}`);
                
                // استخراج أنماط الأشكال من اللوحتين الرئيسية والفرعية
                const mainPattern = extractPatternFromFigure(mainCell.querySelector('.figure-container'));
                const secondaryPattern = extractPatternFromFigure(secondaryCell.querySelector('.figure-container'));
                
                // ضرب الشكلين معاً للحصول على شكل باطن الرمل
                const innerPattern = multiplyFigures(mainPattern, secondaryPattern);
                
                // إزالة الشكل السابق إن وجد
                const oldFigure = innerCell.querySelector('.figure-container');
                if (oldFigure) {
                    oldFigure.remove();
                }
                
                // البحث عن مؤشر الشكل المطابق للنمط
                const figureIndex = findFigureIndexByPattern(innerPattern);
                
                // إنشاء شكل جديد في لوحة باطن الرمل
                const newFigureElement = createFigureElement(innerPattern, figureIndex);
                innerCell.appendChild(newFigureElement);
                
                // تحديث فئة العنصر للخلية
                if (figureIndex !== -1) {
                    innerCell.className = `cell ${geomancyFigures[figureIndex].element}-element`;
                } else {
                    // إذا لم يتم العثور على الشكل، استخدم الفئة بناءً على النمط
                    innerCell.className = `cell ${getElementClassByPattern(innerPattern)}`;
                }
            }
            
            console.log("تم توليد لوحة باطن الرمل بنجاح");
        }

        // حساب مجموع النقاط في شكل واحد
        function countDotsInFigure(pattern) {
            let singleDots = 0;
            let doubleDots = 0;
            
            for (let i = 0; i < pattern.length; i++) {
                if (pattern[i] === 1) {
                    singleDots++;
                } else if (pattern[i] === 2) {
                    doubleDots++;
                }
            }
            
            return { singleDots, doubleDots };
        }

        // حساب مفردات الرمل للسؤال (الأشكال في البيوت الفردية)
        function calculateSandSingles() {
            // التحقق من اكتمال اللوحة الفرعية
            for (let i = 1; i <= 16; i += 2) { // البيوت الفردية فقط
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل الأشكال بعد
                }
            }
            
            // استخراج أنماط الأشكال من البيوت الفردية
            const figure1Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-1').querySelector('.figure-container'));
            const figure3Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-3').querySelector('.figure-container'));
            const figure5Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-5').querySelector('.figure-container'));
            const figure7Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-7').querySelector('.figure-container'));
            const figure9Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-9').querySelector('.figure-container'));
            const figure11Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-11').querySelector('.figure-container'));
            const figure13Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-13').querySelector('.figure-container'));
            const figure15Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-15').querySelector('.figure-container'));
            
            // ضرب الأشكال وفقاً للقواعد
            const result1_3 = multiplyFigures(figure1Pattern, figure3Pattern);
            const result5_7 = multiplyFigures(figure5Pattern, figure7Pattern);
            const result9_11 = multiplyFigures(figure9Pattern, figure11Pattern);
            const result13_15 = multiplyFigures(figure13Pattern, figure15Pattern);
            
            // ضرب النتائج الوسيطة
            const result1_3_5_7 = multiplyFigures(result1_3, result5_7);
            const result9_11_13_15 = multiplyFigures(result9_11, result13_15);
            
            // الناتج النهائي
            const finalResult = multiplyFigures(result1_3_5_7, result9_11_13_15);
            
            // البحث عن اسم الشكل الناتج
            const figureIndex = findFigureIndexByPattern(finalResult);
            
            return {
                pattern: finalResult,
                figureIndex: figureIndex,
                name: figureIndex !== -1 ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب مزدوجات الرمل للجواب (الأشكال في البيوت الزوجية)
        function calculateSandDoubles() {
            // التحقق من اكتمال اللوحة الفرعية
            for (let i = 2; i <= 16; i += 2) { // البيوت الزوجية فقط
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل الأشكال بعد
                }
            }
            
            // استخراج أنماط الأشكال من البيوت الزوجية
            const figure2Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-2').querySelector('.figure-container'));
            const figure4Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-4').querySelector('.figure-container'));
            const figure6Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-6').querySelector('.figure-container'));
            const figure8Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-8').querySelector('.figure-container'));
            const figure10Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-10').querySelector('.figure-container'));
            const figure12Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-12').querySelector('.figure-container'));
            const figure14Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-14').querySelector('.figure-container'));
            const figure16Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-16').querySelector('.figure-container'));
            
            // ضرب الأشكال وفقاً للقواعد
            const result2_4 = multiplyFigures(figure2Pattern, figure4Pattern);
            const result6_8 = multiplyFigures(figure6Pattern, figure8Pattern);
            const result10_12 = multiplyFigures(figure10Pattern, figure12Pattern);
            const result14_16 = multiplyFigures(figure14Pattern, figure16Pattern);
            
            // ضرب النتائج الوسيطة
            const result2_4_6_8 = multiplyFigures(result2_4, result6_8);
            const result10_12_14_16 = multiplyFigures(result10_12, result14_16);
            
            // الناتج النهائي
            const finalResult = multiplyFigures(result2_4_6_8, result10_12_14_16);
            
            // البحث عن اسم الشكل الناتج
            const figureIndex = findFigureIndexByPattern(finalResult);
            
            return {
                pattern: finalResult,
                figureIndex: figureIndex,
                name: figureIndex !== -1 ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب الضمير بطريقة مجموع نقاط الرمل مقسومة على 12
        function calculateDameerMethod1() {
            let totalDots = 0;
            
            // التحقق من اكتمال اللوحة الفرعية
            for (let i = 1; i <= 16; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل الأشكال بعد
                }
                
                // استخراج نمط الشكل
                const pattern = extractPatternFromFigure(figureContainer);
                
                // حساب مجموع النقاط المفتوحة (المفردة)
                for (let j = 0; j < pattern.length; j++) {
                    if (pattern[j] === 1) {
                        totalDots++;
                    }
                }
            }
            
            // حساب باقي القسمة على 12
            const remainder = totalDots % 12;
            
            // تحويل الباقي 0 إلى 12
            const houseNumber = remainder === 0 ? 12 : remainder;
            
            // الحصول على الشكل في البيت المحدد
            const cell = document.getElementById(`secondary-cell-${houseNumber}`);
            const figureContainer = cell.querySelector('.figure-container');
            const figureIndex = figureContainer.dataset.figureIndex;
            
            return {
                houseNumber: houseNumber,
                figureName: figureIndex !== undefined ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب الضمير بطريقة ابدح للأمهات
        function calculateDameerMethod2() {
            // التحقق من وجود أشكال الأمهات
            for (let i = 1; i <= 4; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل أشكال الأمهات بعد
                }
            }
            
            let totalAbdah = 0;
            
            // حساب قيمة ابدح لكل شكل من الأمهات
            for (let i = 1; i <= 4; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                const pattern = extractPatternFromFigure(figureContainer);
                
                // حساب قيمة ابدح للشكل
                // الألف (المرتبة الأولى - النار) = 1
                // الباء (المرتبة الثانية - الهواء) = 2
                // الدال (المرتبة الثالثة - الماء) = 4
                // الحاء (المرتبة الرابعة - التراب) = 8
                
                let abdahValue = 0;
                
                // المرتبة الأولى (النار)
                if (pattern[0] === 1) {
                    abdahValue += 1;
                }
                
                // المرتبة الثانية (الهواء)
                if (pattern[1] === 1) {
                    abdahValue += 2;
                }
                
                // المرتبة الثالثة (الماء)
                if (pattern[2] === 1) {
                    abdahValue += 4;
                }
                
                // المرتبة الرابعة (التراب)
                if (pattern[3] === 1) {
                    abdahValue += 8;
                }
                
                totalAbdah += abdahValue;
            }
            
            // حساب باقي القسمة على 12
            const remainder = totalAbdah % 12;
            
            // تحويل الباقي 0 إلى 12
            const houseNumber = remainder === 0 ? 12 : remainder;
            
            // الحصول على الشكل في البيت المحدد
            const cell = document.getElementById(`secondary-cell-${houseNumber}`);
            const figureContainer = cell.querySelector('.figure-container');
            const figureIndex = figureContainer.dataset.figureIndex;
            
            return {
                houseNumber: houseNumber,
                figureName: figureIndex !== undefined ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب الضمير من خلال عناصر الأمهات
        function calculateDameerMethod3() {
            // التحقق من وجود أشكال الأمهات
            for (let i = 1; i <= 4; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل أشكال الأمهات بعد
                }
            }
            
            // استخراج أنماط الأشكال من الأمهات
            const figure1Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-1').querySelector('.figure-container'));
            const figure2Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-2').querySelector('.figure-container'));
            const figure3Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-3').querySelector('.figure-container'));
            const figure4Pattern = extractPatternFromFigure(document.getElementById('secondary-cell-4').querySelector('.figure-container'));
            
            // تكوين شكل جديد من عناصر الأمهات
            const newPattern = [
                figure1Pattern[0], // مرتبة النار من الشكل الأول
                figure2Pattern[1], // مرتبة الهواء من الشكل الثاني
                figure3Pattern[2], // مرتبة الماء من الشكل الثالث
                figure4Pattern[3]  // مرتبة التراب من الشكل الرابع
            ];
            
            // البحث عن الشكل المطابق
            const figureIndex = findFigureIndexByPattern(newPattern);
            
            return {
                pattern: newPattern,
                figureIndex: figureIndex,
                figureName: figureIndex !== -1 ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب الضمير من خلال عنصر النار لجميع الأشكال
        function calculateDameerMethod4() {
            let totalFireDots = 0;
            
            // التحقق من اكتمال اللوحة الفرعية
            for (let i = 1; i <= 16; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل الأشكال بعد
                }
                
                // استخراج نمط الشكل
                const pattern = extractPatternFromFigure(figureContainer);
                
                // حساب مجموع نقاط النار (المرتبة الأولى)
                if (pattern[0] === 1) {
                    totalFireDots++;
                }
            }
            
            // حساب باقي القسمة على 12
            const remainder = totalFireDots % 12;
            
            // تحويل الباقي 0 إلى 12
            const houseNumber = remainder === 0 ? 12 : remainder;
            
            // الحصول على الشكل في البيت المحدد
            const cell = document.getElementById(`secondary-cell-${houseNumber}`);
            const figureContainer = cell.querySelector('.figure-container');
            const figureIndex = figureContainer.dataset.figureIndex;
            
            return {
                houseNumber: houseNumber,
                figureName: figureIndex !== undefined ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب الضمير من خلال نقاط النار والتراب الفردية من البيت 1 إلى 15
        function calculateDameerMethod5() {
            let totalFireEarthDots = 0;
            
            // التحقق من اكتمال اللوحة الفرعية
            for (let i = 1; i <= 16; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (!figureContainer) {
                    return null; // لم تكتمل الأشكال بعد
                }
            }
            
            // حساب مجموع نقاط النار والتراب الفردية من البيت 1 إلى 15 فقط
            for (let i = 1; i <= 15; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                // استخراج نمط الشكل
                const pattern = extractPatternFromFigure(figureContainer);
                
                // حساب مجموع نقاط النار (المرتبة الأولى) الفردية
                if (pattern[0] === 1) {
                    totalFireEarthDots++;
                }
                
                // حساب مجموع نقاط التراب (المرتبة الرابعة) الفردية
                if (pattern[3] === 1) {
                    totalFireEarthDots++;
                }
            }
            
            // حساب باقي القسمة على 12
            const remainder = totalFireEarthDots % 12;
            
            // تحويل الباقي 0 إلى 12
            const houseNumber = remainder === 0 ? 12 : remainder;
            
            // الحصول على الشكل في البيت المحدد
            const cell = document.getElementById(`secondary-cell-${houseNumber}`);
            const figureContainer = cell.querySelector('.figure-container');
            const figureIndex = figureContainer.dataset.figureIndex;
            
            return {
                houseNumber: houseNumber,
                figureName: figureIndex !== undefined ? geomancyFigures[figureIndex].arabicName : "غير معروف"
            };
        }

        // حساب حركة شكل الطالع (الشكل في البيت الأول من اللوحة الفرعية)
        function calculateAscendantMovement() {
            // التحقق من وجود شكل في البيت الأول من اللوحة الفرعية
            const ascendantCell = document.getElementById('secondary-cell-1');
            const ascendantFigureContainer = ascendantCell.querySelector('.figure-container');
            
            if (!ascendantFigureContainer) {
                return null; // لم يتم اختيار شكل الطالع بعد
            }
            
            // الحصول على مؤشر الشكل في البيت الأول
            const ascendantFigureIndex = parseInt(ascendantFigureContainer.dataset.figureIndex);
            
            // حساب عدد الحركات من البيت الأصلي (في اللوحة الرئيسية) إلى البيت الأول (في اللوحة الفرعية)
            // البيت الأصلي هو البيت الذي يحمل نفس رقم الشكل في اللوحة الرئيسية (مؤشر الشكل + 1)
            const originalHouse = ascendantFigureIndex + 1;
            
            // حساب عدد الحركات (من البيت الأصلي إلى البيت الأول)
            // إذا كان البيت الأصلي هو البيت الأول، فلا توجد حركة (0)
            // وإلا، نحسب عدد الحركات من البيت الأصلي إلى البيت الأول
            let movementNumber;
            if (originalHouse === 1) {
                movementNumber = 0; // الشكل في بيته الأصلي
            } else {
                // حساب عدد الحركات من البيت الأصلي إلى البيت الأول في اتجاه عقارب الساعة
                movementNumber = (1 + 16 - originalHouse) % 16 + 1; // إضافة 1 لزيادة حركة الطالع
                if (movementNumber === 0) movementNumber = 16; // إذا كان الناتج 0، فهذا يعني 16 حركة
            }
            
            return {
                figureName: geomancyFigures[ascendantFigureIndex].arabicName,
                movementNumber: movementNumber
            };
        }

        // تحديث إحصائيات اللوحة الفرعية
        function updateStatistics() {
            // إظهار جدول الإحصائيات
            document.getElementById('statistics-container').style.display = 'block';
            
            // متغيرات لتخزين الإحصائيات
            let elementPoints = {
                fire: 0,
                air: 0,
                water: 0,
                earth: 0
            };
            
            let elementFigures = {
                fire: 0,
                air: 0,
                water: 0,
                earth: 0
            };
            
            let figureTypes = {
                outer: 0,
                inner: 0,
                mobile: 0,
                stable: 0
            };
            
            let fortuneTypes = {
                fortunate: 0,
                unfortunate: 0,
                'mixed-fortunate': 0,
                'mixed-unfortunate': 0,
                mixed: 0
            };
            
            let truthTypes = {
                truthful: 0,
                false: 0
            };
            
            let genderTypes = {
                masculine: 0,
                feminine: 0,
                'neutral-masculine': 0,
                'neutral-feminine': 0
            };
            
            let planetTypes = {
                sun: 0,
                moon: 0,
                mars: 0,
                mercury: 0,
                jupiter: 0,
                venus: 0,
                saturn: 0,
                head: 0,
                tail: 0
            };
            
            // متغيرات لتخزين مجموع النقاط
            let totalSingleDots = 0;
            let totalDoubleDots = 0;
            
            // تخزين أسماء الأشكال حسب نوعها
            let truthfulFigureNames = [];
            let falseFigureNames = [];
            let masculineFigureNames = [];
            let feminineFigureNames = [];
            let neutralFigureNames = [];
            
            // حساب عدد نقاط العناصر المفردة ومجموع النقاط
            for (let i = 1; i <= 16; i++) {
                const cell = document.getElementById(`secondary-cell-${i}`);
                const figureContainer = cell.querySelector('.figure-container');
                
                if (figureContainer) {
                    // استخراج نمط الشكل
                    const pattern = extractPatternFromFigure(figureContainer);
                    
                    // حساب مجموع النقاط في الشكل
                    const { singleDots, doubleDots } = countDotsInFigure(pattern);
                    totalSingleDots += singleDots;
                    totalDoubleDots += doubleDots;
                    
                    // إذا كان هناك مؤشر للشكل، استخدمه للإحصائيات الأخرى
                    if (figureContainer.dataset.figureIndex) {
                        const figureIndex = parseInt(figureContainer.dataset.figureIndex);
                        const figure = geomancyFigures[figureIndex];
                        
                        // حساب نقاط العناصر (النقاط المفردة فقط)
                        for (let j = 0; j < figure.pattern.length; j++) {
                            if (figure.pattern[j] === 1) {
                                // زيادة عدد النقاط المفردة للعنصر المناسب
                                if (j === 0) elementPoints.fire++;
                                else if (j === 1) elementPoints.air++;
                                else if (j === 2) elementPoints.water++;
                                else if (j === 3) elementPoints.earth++;
                            }
                        }
                        
                        // حساب عدد الأشكال حسب العناصر
                        elementFigures[figure.element]++;
                        
                        // حساب أنواع الأشكال
                        figureTypes[figure.type]++;
                        
                        // حساب أنواع السعد والنحس
                        if (figure.fortune === 'mixed') {
                            fortuneTypes.mixed++;
                        } else {
                            fortuneTypes[figure.fortune]++;
                        }
                        
                        // حساب أنواع الصدق والكذب
                        truthTypes[figure.truth]++;
                        
                        // تخزين أسماء الأشكال حسب الصدق والكذب
                        if (figure.truth === 'truthful') {
                            truthfulFigureNames.push(figure.arabicName);
                        } else if (figure.truth === 'false') {
                            falseFigureNames.push(figure.arabicName);
                        }
                        
                        // حساب أنواع الجنس
                        if (figure.gender === 'neutral-masculine' || figure.gender === 'neutral-feminine') {
                            // الأشكال الخنثى
                            genderTypes[figure.gender]++;
                            neutralFigureNames.push(figure.arabicName);
                        } else {
                            genderTypes[figure.gender]++;
                            if (figure.gender === 'masculine') {
                                masculineFigureNames.push(figure.arabicName);
                            } else if (figure.gender === 'feminine') {
                                feminineFigureNames.push(figure.arabicName);
                            }
                        }
                        
                        // حساب أنواع الكواكب
                        planetTypes[figure.planet]++;
                    }
                }
            }
            
            // تحديث عناصر واجهة المستخدم بالإحصائيات
            
            // مجموع النقاط
            document.getElementById('total-single-dots').textContent = totalSingleDots;
            document.getElementById('total-double-dots').textContent = totalDoubleDots;
            // تصحيح حساب المجموع الكلي للنقاط: النقاط المفردة + (النقاط المزدوجة × 2)
            document.getElementById('total-all-dots').textContent = totalSingleDots + (totalDoubleDots * 2);
            
            // نقاط العناصر
            document.getElementById('fire-points').textContent = elementPoints.fire;
            document.getElementById('air-points').textContent = elementPoints.air;
            document.getElementById('water-points').textContent = elementPoints.water;
            document.getElementById('earth-points').textContent = elementPoints.earth;
            
            // أشكال العناصر
            document.getElementById('fire-figures').textContent = elementFigures.fire;
            document.getElementById('air-figures').textContent = elementFigures.air;
            document.getElementById('water-figures').textContent = elementFigures.water;
            document.getElementById('earth-figures').textContent = elementFigures.earth;
            
            // أنواع الأشكال
            document.getElementById('outer-figures').textContent = figureTypes.outer;
            document.getElementById('inner-figures').textContent = figureTypes.inner;
            document.getElementById('mobile-figures').textContent = figureTypes.mobile;
            document.getElementById('stable-figures').textContent = figureTypes.stable;
            
            // السعد والنحس
            document.getElementById('fortunate-figures').textContent = fortuneTypes.fortunate;
            document.getElementById('unfortunate-figures').textContent = fortuneTypes.unfortunate;
            document.getElementById('mixed-fortunate-figures').textContent = fortuneTypes['mixed-fortunate'];
            document.getElementById('mixed-unfortunate-figures').textContent = fortuneTypes['mixed-unfortunate'];
            document.getElementById('mixed-figures').textContent = fortuneTypes.mixed;
            
            // الصدق والكذب
            document.getElementById('truthful-figures').textContent = `${truthTypes.truthful} (${truthfulFigureNames.join('، ')})`;
            document.getElementById('false-figures').textContent = `${truthTypes.false} (${falseFigureNames.join('، ')})`;
            
            // الجنس
            document.getElementById('masculine-figures').textContent = `${genderTypes.masculine} (${masculineFigureNames.join('، ')})`;
            document.getElementById('feminine-figures').textContent = `${genderTypes.feminine} (${feminineFigureNames.join('، ')})`;
            document.getElementById('neutral-figures').textContent = `${genderTypes['neutral-masculine'] + genderTypes['neutral-feminine']} (${neutralFigureNames.join('، ')})`;
            
            // الكواكب
            document.getElementById('sun-figures').textContent = planetTypes.sun;
            document.getElementById('moon-figures').textContent = planetTypes.moon;
            document.getElementById('mars-figures').textContent = planetTypes.mars;
            document.getElementById('mercury-figures').textContent = planetTypes.mercury;
            document.getElementById('jupiter-figures').textContent = planetTypes.jupiter;
            document.getElementById('venus-figures').textContent = planetTypes.venus;
            document.getElementById('saturn-figures').textContent = planetTypes.saturn;
            document.getElementById('head-figures').textContent = planetTypes.head;
            document.getElementById('tail-figures').textContent = planetTypes.tail;
            
            // تحديث مفردات ومزدوجات الرمل
            const sandSingles = calculateSandSingles();
            if (sandSingles) {
                document.getElementById('sand-singles-name').textContent = sandSingles.name;
            }
            
            const sandDoubles = calculateSandDoubles();
            if (sandDoubles) {
                document.getElementById('sand-doubles-name').textContent = sandDoubles.name;
            }
            
            // تحديث طرق حساب الضمير
            const dameerMethod1 = calculateDameerMethod1();
            if (dameerMethod1) {
                document.getElementById('dameer-method1').textContent = `${dameerMethod1.figureName} (${dameerMethod1.houseNumber})`;
            }
            
            const dameerMethod2 = calculateDameerMethod2();
            if (dameerMethod2) {
                document.getElementById('dameer-method2').textContent = `${dameerMethod2.figureName} (${dameerMethod2.houseNumber})`;
            }
            
            const dameerMethod3 = calculateDameerMethod3();
            if (dameerMethod3) {
                document.getElementById('dameer-method3').textContent = dameerMethod3.figureName;
            }
            
            const dameerMethod4 = calculateDameerMethod4();
            if (dameerMethod4) {
                document.getElementById('dameer-method4').textContent = `${dameerMethod4.figureName} (${dameerMethod4.houseNumber})`;
            }
            
            const dameerMethod5 = calculateDameerMethod5();
            if (dameerMethod5) {
                document.getElementById('dameer-method5').textContent = `${dameerMethod5.figureName} (${dameerMethod5.houseNumber})`;
            }
            
            // تحديث معلومات الطالع
            const ascendantCell = document.getElementById('secondary-cell-1');
            const ascendantFigureContainer = ascendantCell.querySelector('.figure-container');
            
            if (ascendantFigureContainer && ascendantFigureContainer.dataset.figureIndex) {
                const figureIndex = parseInt(ascendantFigureContainer.dataset.figureIndex);
                const figure = geomancyFigures[figureIndex];
                
                // اسم شكل الطالع
                document.getElementById('ascendant-figure-name').textContent = figure.arabicName;
                
                // كوكب الطالع
                let planetName = "";
                switch (figure.planet) {
                    case 'sun': planetName = "الشمس"; break;
                    case 'moon': planetName = "القمر"; break;
                    case 'mars': planetName = "المريخ"; break;
                    case 'mercury': planetName = "عطارد"; break;
                    case 'jupiter': planetName = "المشتري"; break;
                    case 'venus': planetName = "الزهرة"; break;
                    case 'saturn': planetName = "زحل"; break;
                    case 'head': planetName = "الرأس"; break;
                    case 'tail': planetName = "الذنب"; break;
                    default: planetName = "غير معروف";
                }
                document.getElementById('ascendant-planet').textContent = planetName;
                
                // نوع الطالع
                let typeName = "";
                switch (figure.type) {
                    case 'outer': typeName = "خارج"; break;
                    case 'inner': typeName = "داخل"; break;
                    case 'mobile': typeName = "منقلب"; break;
                    case 'stable': typeName = "ثابت"; break;
                    default: typeName = "غير معروف";
                }
                document.getElementById('ascendant-type').textContent = typeName;
                
                // سعد/نحس الطالع
                let fortuneName = "";
                switch (figure.fortune) {
                    case 'fortunate': fortuneName = "سعيد"; break;
                    case 'unfortunate': fortuneName = "نحيس"; break;
                    case 'mixed-fortunate': fortuneName = "ممتزج سعد"; break;
                    case 'mixed-unfortunate': fortuneName = "ممتزج نحس"; break;
                    case 'mixed': fortuneName = "ممتزج"; break;
                    default: fortuneName = "غير معروف";
                }
                document.getElementById('ascendant-fortune').textContent = fortuneName;
            }
            
            // تحديث حركة شكل الطالع
            const ascendantMovement = calculateAscendantMovement();
            if (ascendantMovement) {
                document.getElementById('ascendant-movement-name').textContent = ascendantMovement.figureName;
                document.getElementById('ascendant-movement-number').textContent = ascendantMovement.movementNumber;
            }
        }
        
        // الحصول على التاريخ والوقت الحاليين بتنسيق مناسب
        function getCurrentDateTime() {
            const now = new Date();
            
            // أسماء أيام الأسبوع بالعربية
            const arabicDays = ["الأحد", "الإثنين", "الثلاثاء", "الأربعاء", "الخميس", "الجمعة", "السبت"];
            const dayName = arabicDays[now.getDay()];
            
            // تنسيق التاريخ والوقت
            const day = now.getDate();
            const month = now.getMonth() + 1;
            const year = now.getFullYear();
            const hours = now.getHours().toString().padStart(2, '0');
            const minutes = now.getMinutes().toString().padStart(2, '0');
            const seconds = now.getSeconds().toString().padStart(2, '0');
            
            return `${dayName} ${day}/${month}/${year} - ${hours}:${minutes}:${seconds}`;
        }
        
        // حفظ اللوحات والإحصائيات كصورة
        function saveAsImage() {
            // التحقق من اكتمال اللوحات
            if (!document.querySelector('#inner-board .figure-container')) {
                alert('يجب توليد لوحة باطن الرمل أولاً');
                return;
            }
            
            // إظهار شاشة التحميل
            document.getElementById('loading-overlay').style.display = 'flex';
            
            // إنشاء عنصر div مؤقت لتجميع اللوحات والإحصائيات
            const tempDiv = document.createElement('div');
            tempDiv.style.backgroundColor = 'white';
            tempDiv.style.padding = '20px';
            tempDiv.style.width = '100%';
            
            // إضافة السؤال والتاريخ
            const questionContainer = document.querySelector('.question-container').cloneNode(true);
            tempDiv.appendChild(questionContainer);
            
            // إضافة اللوحات
            const boardsContainer = document.querySelector('.boards-container').cloneNode(true);
            tempDiv.appendChild(boardsContainer);
            
            // إضافة الإحصائيات
            const statisticsContainer = document.querySelector('.statistics-container').cloneNode(true);
            statisticsContainer.style.display = 'block';
            tempDiv.appendChild(statisticsContainer);
            
            // إضافة حقوق الملكية
            const copyright = document.querySelector('.copyright').cloneNode(true);
            tempDiv.appendChild(copyright);
            
            // إضافة العنصر المؤقت إلى الصفحة (مخفي)
            tempDiv.style.position = 'absolute';
            tempDiv.style.left = '-9999px';
            document.body.appendChild(tempDiv);
            
            // استخدام html2canvas لتحويل العنصر إلى صورة
            html2canvas(tempDiv, {
                allowTaint: true,
                useCORS: true,
                scale: 3, // تحسين الجودة (زيادة من 2 إلى 3)
                backgroundColor: '#ffffff',
                logging: false,
                letterRendering: true,
                imageTimeout: 0
            }).then(canvas => {
                // تحويل الصورة إلى URL
                const imageURL = canvas.toDataURL('image/png', 1.0); // تحسين جودة الصورة
                
                // إنشاء رابط تنزيل
                const downloadLink = document.createElement('a');
                downloadLink.href = imageURL;
                
                // استخدام السؤال كاسم للملف إذا كان موجوداً، وإلا استخدام التاريخ
                const question = document.getElementById('question-input').value.trim();
                const fileName = question ? 
                    `لوحة_الرمل_${question.substring(0, 30).replace(/[^\w\s]/gi, '')}.png` : 
                    `لوحة_الرمل_${new Date().toISOString().slice(0, 10)}.png`;
                
                downloadLink.download = fileName;
                
                // النقر على الرابط لبدء التنزيل
                downloadLink.click();
                
                // إزالة العنصر المؤقت
                document.body.removeChild(tempDiv);
                
                // إخفاء شاشة التحميل
                document.getElementById('loading-overlay').style.display = 'none';
            }).catch(error => {
                console.error('حدث خطأ أثناء إنشاء الصورة:', error);
                alert('حدث خطأ أثناء إنشاء الصورة. يرجى المحاولة مرة أخرى.');
                // إخفاء شاشة التحميل في حالة الخطأ
                document.getElementById('loading-overlay').style.display = 'none';
            });
        }

        // تنفيذ إنشاء اللوحات عند تحميل الصفحة
        window.onload = function() {
            // إضافة الأسماء العربية للأشكال
            addArabicNamesToFigures();
            
            // إنشاء اللوحات
            createBoards();
            
            // إضافة حدث النقر على زر توليد لوحة باطن الرمل
            document.getElementById('generate-inner-board').addEventListener('click', generateInnerBoard);
            
            // إضافة حدث النقر على زر حفظ اللوحات كصورة
            document.getElementById('save-as-image').addEventListener('click', saveAsImage);
        };
    </script>
<footer-btn></footer-btn>
<script>
    // 定义自定义元素
    class FooterBtn extends HTMLElement {
        constructor() {
            super();
            // 创建 Shadow DOM
            const shadow = this.attachShadow({ mode: 'open' });
            // 创建组件的HTML结构
            const wrapper = document.createElement('div');
            wrapper.setAttribute('class', 'wrapper');
            // 添加一些内容到组件中
            wrapper.innerHTML = `
                <div class="page-footer">
                    <div class="tooltip-dialog">Not an official Manus website nor endorsed by Manus. Content may contain inaccuracies; double-check before use. Do not submit passwords, credit cards, or personal information.</div>
                    <div class="footer-close-btn">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 16 16" fill="none">
                        <g clip-path="url(#clip0_5850_97234)">
                        <path fill-rule="evenodd" clip-rule="evenodd" d="M0.733398 8.00104C0.733398 3.98777 3.9868 0.734375 8.00006 0.734375C12.0133 0.734375 15.2667 3.98777 15.2667 8.00104C15.2667 12.0143 12.0133 15.2677 8.00006 15.2677C3.9868 15.2677 0.733398 12.0143 0.733398 8.00104Z" fill="white"/>
                        <path d="M1.2334 8.00104C1.2334 4.26391 4.26294 1.23438 8.00006 1.23438C11.7372 1.23438 14.7667 4.26391 14.7667 8.00104C14.7667 11.7382 11.7372 14.7677 8.00006 14.7677C4.26294 14.7677 1.2334 11.7382 1.2334 8.00104Z" stroke="black" stroke-opacity="0.14" stroke-linecap="round" stroke-linejoin="round"/>
                        <path fill-rule="evenodd" clip-rule="evenodd" d="M5.57576 5.57613C5.81007 5.34181 6.18997 5.34181 6.42429 5.57613L8.00002 7.15186L9.57576 5.57613C9.81007 5.34181 10.19 5.34181 10.4243 5.57613C10.6586 5.81044 10.6586 6.19034 10.4243 6.42465L8.84855 8.00039L10.4243 9.57613C10.6586 9.81044 10.6586 10.1903 10.4243 10.4247C10.19 10.659 9.81007 10.659 9.57576 10.4247L8.00002 8.84892L6.42429 10.4247C6.18997 10.659 5.81007 10.659 5.57576 10.4247C5.34145 10.1903 5.34145 9.81044 5.57576 9.57613L7.1515 8.00039L5.57576 6.42465C5.34145 6.19034 5.34145 5.81044 5.57576 5.57613Z" fill="#858481"/>
                        </g>
                        <defs>
                        <clipPath id="clip0_5850_97234">
                        <rect width="16" height="16" fill="white"/>
                        </clipPath>
                        </defs>
                        </svg>
                    </div>
                    <div class="footer-bg">
                        <div class="tooltip-box">
                            <div class="tooltip-desc">This user-generated website was created with AI. Use with caution.</div>
                            <span
                                class="tooltip-icon"
                            >
                                <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 12 12" fill="none">
                                    <g clip-path="url(#clip0_4345_83044)">
                                        <path
                                            d="M6 8V6M6 4H6.005M11 6C11 8.76142 8.76142 11 6 11C3.23858 11 1 8.76142 1 6C1 3.23858 3.23858 1 6 1C8.76142 1 11 3.23858 11 6Z"
                                            stroke="#858481"
                                            stroke-linecap="round"
                                            stroke-linejoin="round"
                                        />
                                    </g>
                                    <defs>
                                        <clipPath id="clip0_4345_83044">
                                            <rect width="12" height="12" fill="white" />
                                        </clipPath>
                                    </defs>
                                </svg>
                            </span>
                        </div>
                    </div>
                    <a href="https://manus.im/invitation?from=space" target="_blank" class="footer-button">
                        <span class="footer-logo">
                            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18" fill="none">
                                <path
                                    d="M13.2251 16.9379C14.4717 16.147 15.3336 11.9052 15.144 10.6421C15.144 10.6421 14.9464 9.96396 14.4643 9.96394C13.9821 9.96391 13.7815 10.4719 13.7815 10.4719C13.7815 10.4719 13.6546 10.9409 13.4555 11.5313C13.1474 12.4447 13.0178 13.5718 12.2143 13.8571C11.4107 14.1425 10.6071 13.8725 10.6071 13.8725C11.1535 15.9448 11.9785 17.7288 13.2251 16.9379Z"
                                    fill="white"
                                />
                                <path
                                    fill-rule="evenodd"
                                    clip-rule="evenodd"
                                    d="M7.04826 9.17689C6.59029 9.22824 6.34906 9.40136 6.26768 9.48274C5.94625 9.80417 5.78554 10.1256 5.78554 10.2863C5.78554 10.447 5.40745 9.53393 5.32437 9.44837C5.07703 9.19366 5.083 8.78667 5.33771 8.53933C5.69527 8.19212 6.28003 7.96925 6.90502 7.89918C7.55323 7.82651 8.3219 7.9075 9.09971 8.25205C10.0075 8.65415 10.4937 9.50199 10.6773 10.2768C10.7705 10.6701 10.7943 11.0743 10.7466 11.4426C10.7002 11.8002 10.5781 12.1826 10.3242 12.4736C9.79525 13.0799 9.01508 13.0116 8.593 12.9221C8.35518 12.8716 8.14542 12.7975 7.99786 12.738C7.92303 12.7079 7.86158 12.6804 7.81747 12.6597C7.79535 12.6493 7.77743 12.6406 7.7642 12.6341C7.75757 12.6308 7.75211 12.628 7.74786 12.6259L7.7424 12.6231L7.74037 12.622L7.73915 12.6214C7.73915 12.6214 7.7388 12.6212 8.03568 12.051L7.7388 12.6212C7.42389 12.4573 7.30152 12.069 7.46548 11.7541C7.62912 11.4398 8.01612 11.3173 8.3307 11.4798M8.3307 11.4798L8.33145 11.4802L8.3307 11.4798ZM8.3307 11.4798L8.33531 11.4822C8.34052 11.4847 8.3498 11.4893 8.36271 11.4953C8.38864 11.5075 8.42852 11.5254 8.4788 11.5457C8.58146 11.5871 8.71705 11.6341 8.85982 11.6644C9.19892 11.7363 9.32081 11.668 9.3554 11.6284C9.38325 11.5965 9.44396 11.4899 9.47154 11.2773C9.49771 11.0755 9.48663 10.8281 9.42625 10.5733C9.30267 10.0519 9.00685 9.61712 8.57899 9.42759C8.02117 9.1805 7.48301 9.12815 7.04826 9.17689"
                                    fill="white"
                                />
                                <path
                                    fill-rule="evenodd"
                                    clip-rule="evenodd"
                                    d="M2.79812 2.53348C2.64176 2.85224 2.7734 3.2374 3.09216 3.39376C3.55 3.61834 3.94731 3.83979 4.34068 4.28331C4.57627 4.54892 4.98258 4.57326 5.2482 4.33767C5.51381 4.10208 5.53815 3.69577 5.30256 3.43016C4.742 2.79815 4.16499 2.48794 3.65839 2.23944C3.33963 2.08308 2.95448 2.21473 2.79812 2.53348Z"
                                    fill="white"
                                />
                                <path
                                    fill-rule="evenodd"
                                    clip-rule="evenodd"
                                    d="M6.27275 0.662519C5.92831 0.748629 5.71889 1.09766 5.805 1.4421C5.86516 1.68274 5.92127 1.8885 5.97263 2.07686C6.07093 2.43738 6.15185 2.73415 6.21054 3.08969C6.26837 3.43999 6.59923 3.67708 6.94952 3.61926C7.29982 3.56143 7.53692 3.23058 7.47909 2.88028C7.40972 2.46006 7.3017 2.06246 7.19328 1.66338C7.14526 1.48666 7.09717 1.30966 7.05233 1.13027C6.96622 0.785827 6.61719 0.57641 6.27275 0.662519Z"
                                    fill="white"
                                />
                                <path
                                    d="M8.99979 3.21456C9.16051 2.57171 9.32122 2.08956 9.61377 1.60742"
                                    stroke="white"
                                    stroke-width="1.28571"
                                    stroke-miterlimit="10"
                                    stroke-linecap="round"
                                />
                                <path
                                    fill-rule="evenodd"
                                    clip-rule="evenodd"
                                    d="M11.3613 6.26832C12.6366 6.52316 12.6368 6.52221 12.6368 6.52221L12.637 6.52126L12.6373 6.51931L12.6381 6.51527L12.6396 6.50658C12.6407 6.50054 12.6418 6.49395 12.6429 6.48687C12.6452 6.47271 12.6476 6.45641 12.65 6.43816C12.6548 6.40191 12.6596 6.35662 12.6628 6.30419C12.6689 6.20315 12.6702 6.05635 12.6443 5.88608C12.5954 5.56354 12.4075 4.95215 11.7455 4.59774L11.6146 4.52762L11.4715 4.48801L11.1424 5.7144C11.4358 5.8715 11.3613 6.26832 11.3613 6.26832ZM11.1573 7.07742L11.3613 6.26832L12.6366 6.52316L12.6296 6.56049L12.4237 7.37732C12.3841 7.57807 12.3789 7.69285 12.381 7.75349C12.3828 7.80284 12.3892 7.81904 12.3924 7.82698L12.3927 7.8279C12.4008 7.84841 12.4216 7.89289 12.4977 8.00832C12.5287 8.05537 12.561 8.10252 12.6021 8.1626C12.613 8.1785 12.6245 8.19531 12.6367 8.21327C12.6919 8.29416 12.7565 8.38953 12.825 8.49621C13.2527 9.16281 13.2947 9.80259 13.2925 10.2126C13.2924 10.2186 13.2924 10.2246 13.2923 10.2305C13.3435 10.2481 13.3978 10.2658 13.4555 10.2841C13.4745 10.2902 13.4997 10.2982 13.5262 10.3065C13.5634 10.3183 13.6031 10.3308 13.6314 10.3399C13.6872 10.3577 13.7584 10.381 13.8272 10.4067C13.8751 10.4245 13.9211 10.4423 13.9644 10.4603C14.0059 10.4775 14.0566 10.4995 14.1099 10.5264C14.1513 10.5473 14.2186 10.5831 14.2948 10.6359L14.9647 10.9242L14.955 11.7823C14.9493 12.2868 14.7463 12.8187 14.6023 13.148C14.4436 13.5111 14.2339 13.8983 14.0408 14.1556C13.7712 14.515 13.4657 14.8945 13.0981 15.1372C12.8815 15.2802 12.6198 15.394 12.3111 15.4313C12.0361 15.4645 11.7981 15.4277 11.6232 15.3851L11.5717 15.3745C11.5199 15.3639 11.4456 15.3486 11.3553 15.3298C11.1748 15.2922 10.9287 15.2404 10.6678 15.1837C10.1867 15.0793 9.53471 14.9331 9.22715 14.8311L9.20492 14.8237L9.18296 14.8155C9.04655 14.7649 8.81591 14.7081 8.43387 14.621C8.41077 14.6157 8.38722 14.6103 8.36328 14.6049C8.03371 14.5299 7.6281 14.4376 7.24401 14.3212C6.8462 14.2008 6.35418 14.0216 5.9364 13.7296C5.4979 13.4232 4.99936 12.8829 4.9716 12.0723C4.96744 11.951 4.97023 11.8337 4.97774 11.7224C4.78027 11.5084 4.61454 11.2286 4.53078 10.8703C4.44918 10.5212 4.49431 10.2118 4.55156 9.99459C4.60897 9.7768 4.69617 9.58643 4.76904 9.44413C4.79364 9.3961 4.81915 9.34874 4.8446 9.30298C4.60356 9.22692 4.3313 9.13294 4.06631 9.01999C3.72023 8.87246 3.22918 8.62908 2.86258 8.2362C2.66514 8.0246 2.4584 7.72132 2.37481 7.3248C2.28733 6.90985 2.36002 6.51171 2.53036 6.1708C2.94502 5.34093 3.73706 5.04773 4.44152 5.02237C5.07762 4.99946 5.75449 5.17745 6.36097 5.39016C6.91355 5.58397 7.70443 5.94444 8.3763 6.25842C8.5976 5.89554 8.88932 5.48549 9.16548 5.15432L9.22034 5.08854L9.28382 5.03075C9.75896 4.59828 10.2927 4.45288 10.6994 4.42169C10.8988 4.4064 11.0705 4.41781 11.1989 4.43448C11.2638 4.44291 11.3198 4.45295 11.3652 4.46245C11.388 4.46721 11.4083 4.47188 11.426 4.4762C11.4349 4.47837 11.4431 4.48045 11.4507 4.48243L11.4616 4.48531L11.4666 4.48668L11.4691 4.48735L11.4703 4.48768C11.4703 4.48768 11.4715 4.48801 11.1424 5.7144C11.1424 5.7144 10.6292 5.57232 10.188 5.96175C10.1836 5.96567 10.1792 5.96965 10.1747 5.97368C10.161 5.99009 10.1473 6.0067 10.1336 6.02351C10.1139 6.04768 10.0941 6.07224 10.0743 6.09711C10.0685 6.10441 10.0627 6.11174 10.0569 6.1191C9.61666 6.67768 9.64277 6.91103 8.83914 7.87543C8.83914 7.87543 8.4205 7.68157 8.1701 7.56386C7.9983 7.48311 7.81669 7.39774 7.63164 7.31169C7.03464 7.03411 6.40173 6.74946 5.94628 6.58972C4.82494 6.19643 3.99821 6.14651 3.69645 6.75042C3.29839 7.54707 4.9148 8.00866 5.72705 8.24061C5.8981 8.28945 6.03349 8.32811 6.10689 8.35758C6.52348 8.52479 6.55255 8.86031 6.42848 9.16103C6.35609 9.33651 6.31482 9.43541 6.10706 9.64317C5.78563 9.9646 5.73292 10.3452 5.7943 10.6078C5.82503 10.7392 5.88582 10.8348 5.95809 10.9044C6.15197 11.0909 6.42842 11.0897 6.42842 11.0897C6.42842 11.0897 6.25016 11.5453 6.26759 12.0541C6.27143 12.1661 6.30155 12.268 6.35346 12.3612C6.66163 12.9145 7.73774 13.1596 8.64189 13.3655C9.02162 13.452 9.37102 13.5316 9.62046 13.6242C10.0676 13.7726 11.8925 14.1433 11.8925 14.1433C12.2292 14.2317 12.438 14.1247 12.9919 13.3864C13.2219 13.0797 13.653 12.2123 13.6583 11.7435C13.5884 11.7134 13.5835 11.7029 13.5786 11.6924C13.5718 11.6779 13.565 11.6633 13.3882 11.5974C13.329 11.5754 13.2499 11.5504 13.1593 11.5218C12.7822 11.4029 12.2071 11.2215 12.0534 10.929C11.9788 10.787 11.9846 10.5964 11.9915 10.3736C12.0022 10.0271 12.0153 9.60261 11.732 9.16115C11.6624 9.05276 11.5957 8.95523 11.5333 8.864C11.1781 8.34469 10.9626 8.0296 11.1573 7.07742ZM5.21736 8.64574C5.21818 8.64341 5.21787 8.64394 5.21684 8.64717L5.21736 8.64574ZM13.2859 10.6214C13.2859 10.6214 13.2859 10.6164 13.2849 10.6077C13.2852 10.6173 13.2859 10.6214 13.2859 10.6214Z"
                                    fill="white"
                                />
                            </svg>
                        </span>
                        <div class="title">Made with Manus</div>
                        <div class="create-title">Create my website</div>
                    </a>
                </div>

                <div class="page-footer-mobile">
                    <div class="footer-button">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18" fill="none">
                            <path
                                d="M13.2251 16.9379C14.4717 16.147 15.3336 11.9052 15.144 10.6421C15.144 10.6421 14.9464 9.96396 14.4643 9.96394C13.9821 9.96391 13.7815 10.4719 13.7815 10.4719C13.7815 10.4719 13.6546 10.9409 13.4555 11.5313C13.1474 12.4447 13.0178 13.5718 12.2143 13.8571C11.4107 14.1425 10.6071 13.8725 10.6071 13.8725C11.1535 15.9448 11.9785 17.7288 13.2251 16.9379Z"
                                fill="white"
                            />
                            <path
                                fill-rule="evenodd"
                                clip-rule="evenodd"
                                d="M7.04826 9.17689C6.59029 9.22824 6.34906 9.40136 6.26768 9.48274C5.94625 9.80417 5.78554 10.1256 5.78554 10.2863C5.78554 10.447 5.40745 9.53393 5.32437 9.44837C5.07703 9.19366 5.083 8.78667 5.33771 8.53933C5.69527 8.19212 6.28003 7.96925 6.90502 7.89918C7.55323 7.82651 8.3219 7.9075 9.09971 8.25205C10.0075 8.65415 10.4937 9.50199 10.6773 10.2768C10.7705 10.6701 10.7943 11.0743 10.7466 11.4426C10.7002 11.8002 10.5781 12.1826 10.3242 12.4736C9.79525 13.0799 9.01508 13.0116 8.593 12.9221C8.35518 12.8716 8.14542 12.7975 7.99786 12.738C7.92303 12.7079 7.86158 12.6804 7.81747 12.6597C7.79535 12.6493 7.77743 12.6406 7.7642 12.6341C7.75757 12.6308 7.75211 12.628 7.74786 12.6259L7.7424 12.6231L7.74037 12.622L7.73915 12.6214C7.73915 12.6214 7.7388 12.6212 8.03568 12.051L7.7388 12.6212C7.42389 12.4573 7.30152 12.069 7.46548 11.7541C7.62912 11.4398 8.01612 11.3173 8.3307 11.4798M8.3307 11.4798L8.33145 11.4802L8.3307 11.4798ZM8.3307 11.4798L8.33531 11.4822C8.34052 11.4847 8.3498 11.4893 8.36271 11.4953C8.38864 11.5075 8.42852 11.5254 8.4788 11.5457C8.58146 11.5871 8.71705 11.6341 8.85982 11.6644C9.19892 11.7363 9.32081 11.668 9.3554 11.6284C9.38325 11.5965 9.44396 11.4899 9.47154 11.2773C9.49771 11.0755 9.48663 10.8281 9.42625 10.5733C9.30267 10.0519 9.00685 9.61712 8.57899 9.42759C8.02117 9.1805 7.48301 9.12815 7.04826 9.17689"
                                fill="white"
                            />
                            <path
                                fill-rule="evenodd"
                                clip-rule="evenodd"
                                d="M2.79812 2.53348C2.64176 2.85224 2.7734 3.2374 3.09216 3.39376C3.55 3.61834 3.94731 3.83979 4.34068 4.28331C4.57627 4.54892 4.98258 4.57326 5.2482 4.33767C5.51381 4.10208 5.53815 3.69577 5.30256 3.43016C4.742 2.79815 4.16499 2.48794 3.65839 2.23944C3.33963 2.08308 2.95448 2.21473 2.79812 2.53348Z"
                                fill="white"
                            />
                            <path
                                fill-rule="evenodd"
                                clip-rule="evenodd"
                                d="M6.27275 0.662519C5.92831 0.748629 5.71889 1.09766 5.805 1.4421C5.86516 1.68274 5.92127 1.8885 5.97263 2.07686C6.07093 2.43738 6.15185 2.73415 6.21054 3.08969C6.26837 3.43999 6.59923 3.67708 6.94952 3.61926C7.29982 3.56143 7.53692 3.23058 7.47909 2.88028C7.40972 2.46006 7.3017 2.06246 7.19328 1.66338C7.14526 1.48666 7.09717 1.30966 7.05233 1.13027C6.96622 0.785827 6.61719 0.57641 6.27275 0.662519Z"
                                fill="white"
                            />
                            <path
                                d="M8.99979 3.21456C9.16051 2.57171 9.32122 2.08956 9.61377 1.60742"
                                stroke="white"
                                stroke-width="1.28571"
                                stroke-miterlimit="10"
                                stroke-linecap="round"
                            />
                            <path
                                fill-rule="evenodd"
                                clip-rule="evenodd"
                                d="M11.3613 6.26832C12.6366 6.52316 12.6368 6.52221 12.6368 6.52221L12.637 6.52126L12.6373 6.51931L12.6381 6.51527L12.6396 6.50658C12.6407 6.50054 12.6418 6.49395 12.6429 6.48687C12.6452 6.47271 12.6476 6.45641 12.65 6.43816C12.6548 6.40191 12.6596 6.35662 12.6628 6.30419C12.6689 6.20315 12.6702 6.05635 12.6443 5.88608C12.5954 5.56354 12.4075 4.95215 11.7455 4.59774L11.6146 4.52762L11.4715 4.48801L11.1424 5.7144C11.4358 5.8715 11.3613 6.26832 11.3613 6.26832ZM11.1573 7.07742L11.3613 6.26832L12.6366 6.52316L12.6296 6.56049L12.4237 7.37732C12.3841 7.57807 12.3789 7.69285 12.381 7.75349C12.3828 7.80284 12.3892 7.81904 12.3924 7.82698L12.3927 7.8279C12.4008 7.84841 12.4216 7.89289 12.4977 8.00832C12.5287 8.05537 12.561 8.10252 12.6021 8.1626C12.613 8.1785 12.6245 8.19531 12.6367 8.21327C12.6919 8.29416 12.7565 8.38953 12.825 8.49621C13.2527 9.16281 13.2947 9.80259 13.2925 10.2126C13.2924 10.2186 13.2924 10.2246 13.2923 10.2305C13.3435 10.2481 13.3978 10.2658 13.4555 10.2841C13.4745 10.2902 13.4997 10.2982 13.5262 10.3065C13.5634 10.3183 13.6031 10.3308 13.6314 10.3399C13.6872 10.3577 13.7584 10.381 13.8272 10.4067C13.8751 10.4245 13.9211 10.4423 13.9644 10.4603C14.0059 10.4775 14.0566 10.4995 14.1099 10.5264C14.1513 10.5473 14.2186 10.5831 14.2948 10.6359L14.9647 10.9242L14.955 11.7823C14.9493 12.2868 14.7463 12.8187 14.6023 13.148C14.4436 13.5111 14.2339 13.8983 14.0408 14.1556C13.7712 14.515 13.4657 14.8945 13.0981 15.1372C12.8815 15.2802 12.6198 15.394 12.3111 15.4313C12.0361 15.4645 11.7981 15.4277 11.6232 15.3851L11.5717 15.3745C11.5199 15.3639 11.4456 15.3486 11.3553 15.3298C11.1748 15.2922 10.9287 15.2404 10.6678 15.1837C10.1867 15.0793 9.53471 14.9331 9.22715 14.8311L9.20492 14.8237L9.18296 14.8155C9.04655 14.7649 8.81591 14.7081 8.43387 14.621C8.41077 14.6157 8.38722 14.6103 8.36328 14.6049C8.03371 14.5299 7.6281 14.4376 7.24401 14.3212C6.8462 14.2008 6.35418 14.0216 5.9364 13.7296C5.4979 13.4232 4.99936 12.8829 4.9716 12.0723C4.96744 11.951 4.97023 11.8337 4.97774 11.7224C4.78027 11.5084 4.61454 11.2286 4.53078 10.8703C4.44918 10.5212 4.49431 10.2118 4.55156 9.99459C4.60897 9.7768 4.69617 9.58643 4.76904 9.44413C4.79364 9.3961 4.81915 9.34874 4.8446 9.30298C4.60356 9.22692 4.3313 9.13294 4.06631 9.01999C3.72023 8.87246 3.22918 8.62908 2.86258 8.2362C2.66514 8.0246 2.4584 7.72132 2.37481 7.3248C2.28733 6.90985 2.36002 6.51171 2.53036 6.1708C2.94502 5.34093 3.73706 5.04773 4.44152 5.02237C5.07762 4.99946 5.75449 5.17745 6.36097 5.39016C6.91355 5.58397 7.70443 5.94444 8.3763 6.25842C8.5976 5.89554 8.88932 5.48549 9.16548 5.15432L9.22034 5.08854L9.28382 5.03075C9.75896 4.59828 10.2927 4.45288 10.6994 4.42169C10.8988 4.4064 11.0705 4.41781 11.1989 4.43448C11.2638 4.44291 11.3198 4.45295 11.3652 4.46245C11.388 4.46721 11.4083 4.47188 11.426 4.4762C11.4349 4.47837 11.4431 4.48045 11.4507 4.48243L11.4616 4.48531L11.4666 4.48668L11.4691 4.48735L11.4703 4.48768C11.4703 4.48768 11.4715 4.48801 11.1424 5.7144C11.1424 5.7144 10.6292 5.57232 10.188 5.96175C10.1836 5.96567 10.1792 5.96965 10.1747 5.97368C10.161 5.99009 10.1473 6.0067 10.1336 6.02351C10.1139 6.04768 10.0941 6.07224 10.0743 6.09711C10.0685 6.10441 10.0627 6.11174 10.0569 6.1191C9.61666 6.67768 9.64277 6.91103 8.83914 7.87543C8.83914 7.87543 8.4205 7.68157 8.1701 7.56386C7.9983 7.48311 7.81669 7.39774 7.63164 7.31169C7.03464 7.03411 6.40173 6.74946 5.94628 6.58972C4.82494 6.19643 3.99821 6.14651 3.69645 6.75042C3.29839 7.54707 4.9148 8.00866 5.72705 8.24061C5.8981 8.28945 6.03349 8.32811 6.10689 8.35758C6.52348 8.52479 6.55255 8.86031 6.42848 9.16103C6.35609 9.33651 6.31482 9.43541 6.10706 9.64317C5.78563 9.9646 5.73292 10.3452 5.7943 10.6078C5.82503 10.7392 5.88582 10.8348 5.95809 10.9044C6.15197 11.0909 6.42842 11.0897 6.42842 11.0897C6.42842 11.0897 6.25016 11.5453 6.26759 12.0541C6.27143 12.1661 6.30155 12.268 6.35346 12.3612C6.66163 12.9145 7.73774 13.1596 8.64189 13.3655C9.02162 13.452 9.37102 13.5316 9.62046 13.6242C10.0676 13.7726 11.8925 14.1433 11.8925 14.1433C12.2292 14.2317 12.438 14.1247 12.9919 13.3864C13.2219 13.0797 13.653 12.2123 13.6583 11.7435C13.5884 11.7134 13.5835 11.7029 13.5786 11.6924C13.5718 11.6779 13.565 11.6633 13.3882 11.5974C13.329 11.5754 13.2499 11.5504 13.1593 11.5218C12.7822 11.4029 12.2071 11.2215 12.0534 10.929C11.9788 10.787 11.9846 10.5964 11.9915 10.3736C12.0022 10.0271 12.0153 9.60261 11.732 9.16115C11.6624 9.05276 11.5957 8.95523 11.5333 8.864C11.1781 8.34469 10.9626 8.0296 11.1573 7.07742ZM5.21736 8.64574C5.21818 8.64341 5.21787 8.64394 5.21684 8.64717L5.21736 8.64574ZM13.2859 10.6214C13.2859 10.6214 13.2859 10.6164 13.2849 10.6077C13.2852 10.6173 13.2859 10.6214 13.2859 10.6214Z"
                                fill="white"
                            />
                        </svg>
                        <div class="title">Made with Manus</div>
                        <div class="create-title">Create my website</div>
                    </div>
                    <div class="footer-dialog">
                        <div class="dialog-box">
                            <div class="dialog-header">
                                <div class="dialog-header-title">Made with Manus</div>
                                <span class="dialog-header-icon">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18" fill="none">
                                        <path
                                            fill-rule="evenodd"
                                            clip-rule="evenodd"
                                            d="M3.2729 3.27192C3.5365 3.00832 3.96389 3.00832 4.22749 3.27192L9.0002 8.04462L13.7729 3.27192C14.0365 3.00832 14.4639 3.00832 14.7275 3.27192C14.9911 3.53553 14.9911 3.96291 14.7275 4.22652L9.95479 8.99922L14.7275 13.7719C14.9911 14.0355 14.9911 14.4629 14.7275 14.7265C14.4639 14.9901 14.0365 14.9901 13.7729 14.7265L9.0002 9.95381L4.22749 14.7265C3.96389 14.9901 3.5365 14.9901 3.2729 14.7265C3.00929 14.4629 3.00929 14.0355 3.2729 13.7719L8.0456 8.99922L3.2729 4.22652C3.00929 3.96291 3.00929 3.53553 3.2729 3.27192Z"
                                            fill="#858481"
                                        />
                                    </svg>
                                </span>
                            </div>
                            <div class="dialog-body">
                                <div>This user-generated website was created with AI. Use with caution.</div>
                                <div>
                                    Not an official Manus website nor endorsed by Manus. Content may contain inaccuracies; double-check before use. Do not submit
                                    passwords, credit cards, or personal information.
                                </div>
                            </div>
                            <div class="dialog-footer">
                                <div class="dialog-footer-button-left">Cancel</div>
                                <div class="dialog-footer-button-right" onclick="createSite()">Create my website</div>
                            </div>
                        </div>
                    </div>
                </div>
            `;
            // 创建样式
            const style = document.createElement('style');
            style.textContent = `
            @keyframes changeWidth {
                0% {
                    width: 150px;
                    background-color: transparent;
                }
                100% {
                    width: 574px;
                    background-color: #fff;
                }
            }

            *,
            *::before,
            *::after {
                box-sizing: border-box;
                margin: 0;
                padding: 0;
                border: unset;
            }

            * {
                line-height: 1.2;
                text-shadow: initial;
                font-family: 'SF Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            }

            a {
                text-decoration: unset;
            }

            .page-footer-mobile {
                z-index: 10000;
                display: none;
                position: absolute;
                right: 16px;
                bottom: 16px;
            }

            .page-footer {
                z-index: 10000;
                display: flex;
                padding: 4px 4px 4px 12px;
                align-items: center;
                gap: 16px;
                position: fixed;
                right: 16px;
                bottom: 16px;
                border-radius: 24px;
                border: 1px solid transparent;
                background: transparent;
                transition: 0.3s;
            }

            .footer-close-btn {
                display: none;
                z-index: 10002;
                cursor: pointer;
                position: absolute;
                right: -3px;
                top: -3px;
            }

            .footer-bg {
                overflow: hidden;
                position: absolute;
                right: 0px;
                bottom: 0px;
                height: 38px;
                width: 150px;
                background-color: transparent;
                border-radius: 24px;
            }

            .tooltip-box {
                display: none;
                transition: 0.3s;
                z-index: 10001;
                align-items: center;
                gap: 1px;
            }

            .tooltip-desc {
                color: #858481;
                /* Label/Primary */
                font-size: 12px;
                font-style: normal;
                font-weight: 400;
                line-height: 16px; /* 133.333% */
            }

            .footer-button {
                z-index: 10001;
                right: 16px;
                bottom: 16px;
                user-select: none;
                cursor: pointer;
                display: flex;
                padding: 5px 10px;
                align-items: center;
                height: 30px;
                gap: 2px;
                transition: 0.3s;
                border-radius: 14px;
                border: 1px solid rgba(0, 0, 0, 0.06);
                background: #1a1a19;
                box-shadow: 0px 0px 0px 1px rgba(0, 0, 0, 0.06), 0px 8px 32px 0px rgba(0, 0, 0, 0.06);
            }

            .footer-logo {
                width: 18px;
                height: 18px;
            }

            .footer-button:hover .create-title {
                opacity: 0.8;
            }

            .footer-button:hover .footer-logo {
                opacity: 0.8;
            }

            .page-footer:hover .footer-close-btn {
                display: block;
            }

            .page-footer:hover .create-title {
                display: block;
            }

            .page-footer:hover .title {
                display: none;
            }

            .page-footer:hover .footer-bg {
                animation: changeWidth 0.3s ease-in-out forwards;
                border: 1px solid rgba(0, 0, 0, 0.06);
                background: #fff;
                box-shadow: 0px 0px 0px 1px rgba(0, 0, 0, 0.06), 0px 8px 32px 0px rgba(0, 0, 0, 0.06);
            }

            .page-footer:hover .tooltip-box {
                position: absolute;
                text-wrap-mode: nowrap;
                display: flex;
                left: 10px;
                top: 9px;
            }

            .tooltip-icon {
                position: relative;
                cursor: pointer;
            }

            .tooltip-dialog {
                display: none;
                position: absolute;
                bottom: 154%;
                left: -10%;
                transform: translateX(-50%);
                padding: 10px;
                border-radius: 12px;
                font-size: 12px;
                line-height: 16px;
                background-color: #171818;
                color: #fff;
                max-width: 280px;
                width: max-content;
            }

            .title {
                color: #fff;
                font-size: 12px;
                font-style: normal;
                font-weight: 400;
                line-height: 16px;
            }

            .create-title {
                display: none;
                color: #fff;
                font-size: 12px;
                font-style: normal;
                font-weight: 400;
            }

            .footer-dialog {
                position: fixed;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                background-color: rgba(0, 0, 0, 0.5);
                display: none;
                justify-content: center;
                align-items: center;
            }

            .dialog-box {
                display: flex;
                width: 358px;
                flex-direction: column;
                align-items: flex-start;
                gap: 8px;
                border-radius: 20px;
                border: 1px solid rgba(255, 255, 255, 0.04);
                background: #fff;

                /* menu/main */
                box-shadow: 0px 0px 0px 1px rgba(0, 0, 0, 0.02), 0px 4px 32px 0px rgba(0, 0, 0, 0.16);
                backdrop-filter: blur(40px);
            }

            .dialog-header {
                display: flex;
                padding: 16px 12px 8px 16px;
                align-items: flex-start;
                gap: 16px;
                align-self: stretch;
            }

            .dialog-header-title {
                flex: 1;
                color: #34322d;
                font-size: 18px;
                font-style: normal;
                font-weight: 600;
            }

            .dialog-body {
                display: flex;
                padding: 0px 16px;
                flex-direction: column;
                align-items: flex-start;
                gap: 12px;
                align-self: stretch;
                color: #535350;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 24px;
            }

            .dialog-footer {
                display: flex;
                padding: 16px;
                align-items: flex-start;
                gap: 12px;
                align-self: stretch;
            }

            .dialog-footer-button-left {
                display: flex;
                width: 119px;
                min-width: 114px;
                min-height: 48px;
                padding: 12px 24px;
                justify-content: center;
                align-items: center;
                gap: 6px;
                border-radius: 12px;
                background: rgba(55, 53, 47, 0.06);
                color: #34322d;
                font-size: 16px;
                font-style: normal;
                font-weight: 600;
            }

            .dialog-footer-button-right {
                display: flex;
                min-width: 114px;
                min-height: 48px;
                padding: 12px 24px;
                justify-content: center;
                align-items: center;
                gap: 6px;
                flex: 1 0 0;
                border-radius: 12px;
                background: #1a1a19;
                color: #fff;
                font-size: 16px;
                font-style: normal;
                font-weight: 600;
            }

            @media (max-width: 600px) {
                .page-footer {
                    display: none;
                }
                .page-footer-mobile {
                    display: block;
                }
                .footer-button {
                    position: fixed
                }
            }
            `;
            // 将样式和内容添加到Shadow DOM
            shadow.appendChild(style);
            shadow.appendChild(wrapper);

            // 在connectedCallback中设置事件监听器
            this.setupEventListeners(shadow);
        }
        onclickEvent(shadow, domStr, fn) {
            const clickDom = shadow.querySelector(domStr);
            if (clickDom) {
                clickDom.onclick = (event) => {
                    fn(event, clickDom);
                };
            }
        }
        setupEventListeners(shadow) {
            const changeDialogStyle = (displayStatus) => {
                const dialog = shadow.querySelector('.footer-dialog');
                if (dialog) {
                    dialog.style.display = displayStatus;
                }
            };

            const createSite = () => {
                window.open('https://manus.im/invitation?from=space', '_blank');
                changeDialogStyle('none');
            };

            this.onclickEvent(shadow, '.page-footer-mobile .dialog-footer-button-left', () => {
                changeDialogStyle('none');
            });

            this.onclickEvent(shadow, '.page-footer-mobile .dialog-footer-button-right', () => {
                createSite();
            });

            this.onclickEvent(shadow, '.page-footer-mobile .dialog-header-icon', () => {
                changeDialogStyle('none');
            });

            this.onclickEvent(shadow, '.page-footer-mobile .footer-button', () => {
                changeDialogStyle('flex');
            });

            this.onclickEvent(shadow, '.footer-close-btn', () => {
                localStorage.setItem('embedClosed', 'true');
                shadow.querySelector('.page-footer').style.display = 'none';
                shadow.querySelector('.page-footer-mobile').style.display = 'none';
            });

            this.onclickEvent(shadow, '.page-footer-mobile .footer-dialog', (event, dialog) => {
                if (event.target === dialog) {
                    changeDialogStyle('none');
                }
            });

            const tooltipIcon = shadow.querySelector('.tooltip-icon');
            const tooltipDialog = shadow.querySelector('.tooltip-dialog');

            tooltipIcon.addEventListener('mouseenter', function () {
                tooltipDialog.style.display = 'block';
            });

            tooltipIcon.addEventListener('mouseleave', function () {
                tooltipDialog.style.display = 'none';
            });

            if (localStorage.getItem('embedClosed')) {
                shadow.querySelector('.page-footer').style.display = 'none';
                shadow.querySelector('.page-footer-mobile').style.display = 'none';
            }
        }
    }
    // 注册自定义元素
    customElements.define('footer-btn', FooterBtn);
</script>
<script
			defer
			data-domain="manus.space"
			src="https://plausible.io/js/script.file-downloads.hash.outbound-links.pageview-props.revenue.tagged-events.js"
		></script>
		<script>
			window.plausible =
				window.plausible ||
				function () {
					(window.plausible.q = window.plausible.q || []).push(arguments);
				};
		</script></body>
</html>
