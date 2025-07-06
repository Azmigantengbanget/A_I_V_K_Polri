<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Kepanjangan AI (Artificial Intelligence)?", "id": "Artificial Intelligence." },
  { "en": "Definisi Kecerdasan Buatan (AI)?", "id": "Simulasi kecerdasan manusia oleh mesin." },
  { "en": "Siapa bapak Kecerdasan Buatan?", "id": "John McCarthy." },
  { "en": "Apa itu 'Turing Test'?", "id": "Tes untuk menentukan kecerdasan mesin." },
  { "en": "Cabang utama dari AI?", "id": "Machine Learning dan Deep Learning." },
  { "en": "Kepanjangan ML (Machine Learning)?", "id": "Machine Learning." },
  { "en": "Definisi Machine Learning (ML)?", "id": "Sistem yang belajar dari data." },
  { "en": "Tiga tipe utama ML?", "id": "Supervised, Unsupervised, Reinforcement." },
  { "en": "Apa itu 'Supervised Learning'?", "id": "Belajar dari data yang berlabel." },
  { "en": "Contoh 'Supervised Learning'?", "id": "Klasifikasi email spam." },
  { "en": "Apa itu 'Unsupervised Learning'?", "id": "Belajar dari data tanpa label." },
  { "en": "Contoh 'Unsupervised Learning'?", "id": "Segmentasi pelanggan." },
  { "en": "Apa itu 'Reinforcement Learning'?", "id": "Belajar dari coba-coba (reward)." },
  { "en": "Contoh 'Reinforcement Learning'?", "id": "AI bermain catur atau Go." },
  { "en": "Apa itu 'model' dalam ML?", "id": "Representasi matematis dari pola data." },
  { "en": "Proses melatih 'model'?", "id": "Training." },
  { "en": "Proses menggunakan 'model' terlatih?", "id": "Inference atau prediksi." },
  { "en": "Apa itu 'data training'?", "id": "Data untuk melatih model AI." },
  { "en": "Apa itu 'data testing'?", "id": "Data untuk menguji performa model." },
  { "en": "Apa itu algoritma?", "id": "Sekumpulan aturan untuk pemecahan masalah." },
  { "en": "Kepanjangan DL (Deep Learning)?", "id": "Deep Learning." },
  { "en": "Definisi Deep Learning (DL)?", "id": "ML menggunakan jaringan saraf tiruan." },
  { "en": "Apa itu Visi Komputer (Computer Vision)?", "id": "AI yang 'melihat' dan pahami gambar." },
  { "en": "Unit terkecil gambar digital?", "id": "Pixel (Picture Element)." },
  { "en": "Apa itu resolusi gambar?", "id": "Jumlah piksel (lebar x tinggi)." },
  { "en": "Format warna paling umum?", "id": "RGB (Red, Green, Blue)." },
  { "en": "Representasi gambar hitam putih?", "id": "Grayscale." },
  { "en": "Nilai piksel pada grayscale?", "id": "0 (hitam) hingga 255 (putih)." },
  { "en": "Apa itu 'image processing'?", "id": "Memanipulasi gambar untuk tujuan tertentu." },
  { "en": "Beda 'computer vision' & 'image processing'?", "id": "Vision memahami, processing memanipulasi." },
  { "en": "Apa itu 'filtering' gambar?", "id": "Mengubah nilai piksel berdasarkan tetangganya." },
  { "en": "Contoh 'filter' gambar?", "id": "Blur, Sharpen, Edge Detection." },
  { "en": "Apa itu 'thresholding'?", "id": "Mengubah gambar jadi hitam putih." },
  { "en": "Apa itu 'edge detection'?", "id": "Mengidentifikasi batas atau tepi objek." },
  { "en": "Kepanjangan GPU (Graphics Processing Unit)?", "id": "Graphics Processing Unit." },
  { "en": "Peran GPU (Graphics Processing Unit) dalam AI?", "id": "Mempercepat pelatihan model Deep Learning." },
  { "en": "Format gambar populer?", "id": "JPEG, PNG, BMP." },
  { "en": "Kompresi pada format JPEG?", "id": "Lossy compression." },
  { "en": "Kompresi pada format PNG?", "id": "Lossless compression." },
  { "en": "Apa itu 'neural network'?", "id": "Jaringan saraf tiruan." },
  { "en": "Unit dasar 'neural network'?", "id": "Neuron atau perceptron." },
  { "en": "Lapisan pada 'neural network'?", "id": "Input, hidden, dan output layer." },
  { "en": "Fungsi 'activation function'?", "id": "Menentukan output dari sebuah neuron." },
  { "en": "Contoh 'activation function'?", "id": "ReLU, Sigmoid, Tanh." },
  { "en": "Apa itu 'weights'?", "id": "Parameter yang dipelajari oleh model." },
  { "en": "Apa itu 'bias'?", "id": "Parameter untuk menggeser output." },
  { "en": "Apa itu 'backpropagation'?", "id": "Algoritma untuk melatih neural network." },
  { "en": "Tugas klasifikasi (classification)?", "id": "Memprediksi kategori atau kelas." },
  { "en": "Tugas regresi (regression)?", "id": "Memprediksi nilai numerik kontinu." },
  { "en": "Contoh tugas klasifikasi?", "id": "Kucing atau anjing." },
  { "en": "Contoh tugas regresi?", "id": "Memprediksi harga rumah." },
  { "en": "Apa itu 'overfitting'?", "id": "Model terlalu hafal data training." },
  { "en": "Dampak 'overfitting'?", "id": "Performa buruk pada data baru." },
  { "en": "Apa itu 'underfitting'?", "id": "Model terlalu sederhana, gagal belajar." },
  { "en": "Metrik evaluasi klasifikasi?", "id": "Akurasi, presisi, recall, F1-score." },
  { "en": "Apa itu 'confusion matrix'?", "id": "Tabel untuk evaluasi performa klasifikasi." },
  { "en": "Apa itu 'True Positive' (TP)?", "id": "Prediksi positif yang benar." },
  { "en": "Apa itu 'True Negative' (TN)?", "id": "Prediksi negatif yang benar." },
  { "en": "Apa itu 'False Positive' (FP)?", "id": "Prediksi positif yang salah." },
  { "en": "Apa itu 'False Negative' (FN)?", "id": "Prediksi negatif yang salah." },
  { "en": "Algoritma 'Decision Tree'?", "id": "Model klasifikasi berbentuk struktur pohon." },
  { "en": "Kepanjangan SVM (Support Vector Machine)?", "id": "Support Vector Machine." },
  { "en": "Kepanjangan KNN (K-Nearest Neighbors)?", "id": "K-Nearest Neighbors." },
  { "en": "Apa itu 'clustering'?", "id": "Mengelompokkan data tanpa label." },
  { "en": "Algoritma 'clustering' populer?", "id": "K-Means." },
  { "en": "Apa itu 'dimensionality reduction'?", "id": "Mengurangi jumlah fitur data." },
  { "en": "Algoritma 'dimensionality reduction' populer?", "id": "PCA (Principal Component Analysis)." },
  { "en": "Kepanjangan CNN (Convolutional Neural Network)?", "id": "Convolutional Neural Network." },
  { "en": "CNN (Convolutional Neural Network) khusus untuk data?", "id": "Data gambar dan video." },
  { "en": "Lapisan utama pada CNN?", "id": "Convolutional, Pooling, Fully Connected." },
  { "en": "Fungsi 'convolutional layer'?", "id": "Mengekstrak fitur dari gambar." },
  { "en": "Fungsi 'pooling layer'?", "id": "Mengurangi ukuran dimensi fitur." },
  { "en": "Apa itu 'feature map'?", "id": "Output dari convolutional layer." },
  { "en": "Apa itu 'Image Classification'?", "id": "Memberi label pada seluruh gambar." },
  { "en": "Apa itu 'Object Detection'?", "id": "Menemukan lokasi dan kelas objek." },
  { "en": "Output 'object detection'?", "id": "Bounding box dan label kelas." },
  { "en": "Apa itu 'bounding box'?", "id": "Kotak yang melingkupi objek." },
  { "en": "Algoritma 'object detection'?", "id": "R-CNN, YOLO, SSD." },
  { "en": "Kepanjangan YOLO?", "id": "You Only Look Once." },
  { "en": "Apa itu 'Image Segmentation'?", "id": "Memberi label pada setiap piksel." },
  { "en": "Apa itu 'semantic segmentation'?", "id": "Memberi label kelas pada piksel." },
  { "en": "Apa itu 'instance segmentation'?", "id": "Bedakan tiap objek dalam kelas sama." },
  { "en": "Apa itu 'transfer learning'?", "id": "Menggunakan model terlatih untuk tugas baru." },
  { "en": "Keuntungan 'transfer learning'?", "id": "Menghemat waktu dan data training." },
  { "en": "Kepanjangan NLP (Natural Language Processing)?", "id": "Natural Language Processing." },
  { "en": "Tujuan NLP (Natural Language Processing)?", "id": "Membuat komputer paham bahasa manusia." },
  { "en": "Apa itu 'Generative AI'?", "id": "AI yang bisa menciptakan konten baru." },
  { "en": "Kepanjangan GAN (Generative Adversarial Network)?", "id": "Generative Adversarial Network." },
  { "en": "Prinsip kerja GAN (Generative Adversarial Network)?", "id": "Dua jaringan: generator dan diskriminator." },
  { "en": "Apa itu 'Facial Recognition'?", "id": "Teknologi identifikasi wajah." },
  { "en": "Apa itu 'Pose Estimation'?", "id": "Mendeteksi posisi dan orientasi tubuh." },
  { "en": "Kepanjangan OCR (Optical Character Recognition)?", "id": "Optical Character Recognition." },
  { "en": "Fungsi OCR (Optical Character Recognition)?", "id": "Membaca teks dari gambar." },
  { "en": "Aplikasi OCR (Optical Character Recognition) Polri?", "id": "Membaca plat nomor kendaraan." },
  { "en": "Video atau audio manipulasi AI?", "id": "Deepfake." },
  { "en": "Ancaman 'deepfake'?", "id": "Penyebaran hoaks dan disinformasi." },
  { "en": "Kepanjangan RNN (Recurrent Neural Network)?", "id": "Recurrent Neural Network." },
  { "en": "RNN (Recurrent Neural Network) cocok untuk data?", "id": "Data sekuensial seperti teks." },
  { "en": "Kepanjangan LSTM (Long Short-Term Memory)?", "id": "Long Short-Term Memory." },
  { "en": "Fungsi LSTM (Long Short-Term Memory)?", "id": "Jenis RNN untuk dependensi jangka panjang." },
  { "en": "Apa itu 'agent' dalam AI?", "id": "Entitas yang beraksi di lingkungan." },
  { "en": "Apa itu 'environment'?", "id": "Lingkungan tempat agent berinteraksi." },
  { "en": "Apa itu 'reward'?", "id": "Sinyal umpan balik untuk agent." },
  { "en": "Siklus hidup pengembangan ML?", "id": "MLOps (Machine Learning Operations)." },
  { "en": "Apa itu 'data augmentation'?", "id": "Memperbanyak data training secara sintetis." },
  { "en": "Tujuan 'data augmentation'?", "id": "Mencegah overfitting, tingkatkan performa." },
  { "en": "Hardware akselerator AI?", "id": "GPU, TPU, ASIC." },
  { "en": "Kepanjangan TPU (Tensor Processing Unit)?", "id": "Tensor Processing Unit." },
  { "en": "Apa itu 'bias' dalam AI?", "id": "Kecenderungan AI tidak adil." },
  { "en": "Penyebab 'bias' AI?", "id": "Data training yang tidak seimbang." },
  { "en": "Kepanjangan XAI (Explainable AI)?", "id": "Explainable AI." },
  { "en": "Tujuan XAI (Explainable AI)?", "id": "Membuat keputusan AI dapat dijelaskan." },
  { "en": "Apa itu 'data anonymization'?", "id": "Menghapus informasi identitas dari data." },
  { "en": "Apa itu 'attention mechanism'?", "id": "Mekanisme AI untuk fokus pada data." },
  { "en": "Arsitektur model 'Transformer'?", "id": "Berbasis 'self-attention mechanism'." },
  { "en": "Model 'Transformer' populer?", "id": "BERT, GPT." },
  { "en": "Apa itu 'one-shot learning'?", "id": "Belajar dari satu contoh saja." },
  { "en": "Apa itu 'zero-shot learning'?", "id": "Mengenali objek tanpa melihat contoh." },
  { "en": "Apa itu 'federated learning'?", "id": "Melatih model tanpa kumpulkan data." },
  { "en": "Manfaat 'federated learning'?", "id": "Menjaga privasi data pengguna." },
  { "en": "Apa itu 'adversarial attack'?", "id": "Serangan untuk menipu model AI." },
  { "en": "Data dari sensor LiDAR?", "id": "Point cloud (awan titik)." },
  { "en": "Apa itu 'multimodal AI'?", "id": "AI yang proses banyak jenis data." },
  { "en": "Contoh 'multimodal AI'?", "id": "Memproses gambar dan teks bersamaan." },
  { "en": "Apa itu 'feature' dalam ML?", "id": "Atribut atau variabel input." },
  { "en": "Apa itu 'label'?", "id": "Output atau jawaban yang benar." },
  { "en": "Apa itu 'loss function'?", "id": "Fungsi untuk mengukur error model." },
  { "en": "Apa itu 'optimizer'?", "id": "Algoritma untuk meminimalkan loss." },
  { "en": "Contoh 'optimizer'?", "id": "Adam, SGD (Stochastic Gradient Descent)." },
  { "en": "Apa itu 'learning rate'?", "id": "Ukuran langkah pembaruan 'weights'." },
  { "en": "Apa itu 'epoch'?", "id": "Satu siklus penuh data training." },
  { "en": "Apa itu 'batch size'?", "id": "Jumlah sampel data per iterasi." },
  { "en": "Apa itu 'hyperparameter'?", "id": "Parameter yang diatur sebelum training." },
  { "en": "Contoh 'hyperparameter'?", "id": "Learning rate, batch size, jumlah layer." },
  { "en": "Apa itu 'hyperparameter tuning'?", "id": "Proses mencari hyperparameter terbaik." },
  { "en": "Ruang warna untuk video?", "id": "YUV atau YCbCr." },
  { "en": "Proses pemisahan warna gambar?", "id": "Color space transformation." },
  { "en": "Apa itu 'histogram' gambar?", "id": "Grafik distribusi intensitas piksel." },
  { "en": "Fungsi 'histogram equalization'?", "id": "Meningkatkan kontras gambar secara otomatis." },
  { "en": "Apa itu 'kernel' atau 'filter'?", "id": "Matriks kecil untuk operasi konvolusi." },
  { "en": "Operasi morfologi gambar?", "id": "Erosion, Dilation, Opening, Closing." },
  { "en": "Fungsi 'erosion'?", "id": "Mengecilkan atau menipiskan objek putih." },
  { "en": "Fungsi 'dilation'?", "id": "Menebalkan atau memperbesar objek putih." },
  { "en": "Apa itu 'feature extraction'?", "id": "Proses mengekstrak ciri khas data." },
  { "en": "Contoh 'feature descriptor'?", "id": "SIFT, SURF, ORB." },
  { "en": "Apa itu 'data preprocessing'?", "id": "Tahap persiapan data sebelum training." },
  { "en": "Contoh 'data preprocessing'?", "id": "Normalisasi, standarisasi, cleaning." },
  { "en": "Apa itu 'normalization'?", "id": "Mengubah skala data ke rentang 0-1." },
  { "en": "Apa itu 'data cleaning'?", "id": "Menangani data yang hilang/salah." },
  { "en": "Apa itu 'regression line'?", "id": "Garis terbaik yang cocok data." },
  { "en": "Metode evaluasi regresi?", "id": "MAE, MSE, RMSE, R-squared." },
  { "en": "Kepanjangan MAE (Mean Absolute Error)?", "id": "Mean Absolute Error." },
  { "en": "Kepanjangan MSE (Mean Squared Error)?", "id": "Mean Squared Error." },
  { "en": "Apa itu 'regularization'?", "id": "Teknik untuk mencegah overfitting." },
  { "en": "Jenis 'regularization'?", "id": "L1 (Lasso) dan L2 (Ridge)." },
  { "en": "Apa itu 'dropout'?", "id": "Teknik regularisasi pada neural network." },
  { "en": "Apa itu 'ensemble learning'?", "id": "Menggabungkan beberapa model jadi satu." },
  { "en": "Contoh 'ensemble learning'?", "id": "Random Forest, Gradient Boosting." },
  { "en": "Apa itu 'principal component'?", "id": "Fitur baru hasil dari PCA." },
  { "en": "Apa itu 'centroid' di K-Means?", "id": "Titik pusat dari sebuah cluster." },
  { "en": "Apa itu 'dendrogram'?", "id": "Diagram pohon untuk hierarchical clustering." },
  { "en": "Kelemahan K-Means?", "id": "Harus menentukan jumlah cluster (K)." },
  { "en": "Apa itu 'activation map'?", "id": "Sinonim untuk 'feature map'." },
  { "en": "Apa itu 'stride' konvolusi?", "id": "Ukuran langkah pergeseran kernel." },
  { "en": "Apa itu 'padding' konvolusi?", "id": "Menambahkan piksel di sekitar gambar." },
  { "en": "Tujuan 'padding'?", "id": "Menjaga ukuran, proses piksel tepi." },
  { "en": "Jenis 'pooling'?", "id": "Max Pooling, Average Pooling." },
  { "en": "Apa itu 'flattening'?", "id": "Mengubah matriks menjadi vektor." },
  { "en": "Model CNN (Convolutional Neural Network) terkenal?", "id": "AlexNet, VGG, ResNet, Inception." },
  { "en": "Apa itu 'residual connection'?", "id": "Koneksi pintas pada ResNet." },
  { "en": "Apa itu 'anchor boxes'?", "id": "Kotak referensi pada object detection." },
  { "en": "Apa itu 'IoU (Intersection over Union)'?", "id": "Metrik akurasi untuk bounding box." },
  { "en": "Apa itu 'non-maximum suppression' (NMS)?", "id": "Menghapus bounding box yang tumpang tindih." },
  { "en": "Apa itu 'tokenization' di NLP?", "id": "Memecah teks menjadi kata/kalimat." },
  { "en": "Apa itu 'stop words'?", "id": "Kata umum yang sering diabaikan." },
  { "en": "Contoh 'stop words'?", "id": "Yang, di, dan, dari." },
  { "en": "Apa itu 'stemming'?", "id": "Mengubah kata menjadi kata dasar." },
  { "en": "Apa itu 'lemmatization'?", "id": "Mengubah kata ke bentuk kamus." },
  { "en": "Representasi teks sebagai angka?", "id": "Word embedding." },
  { "en": "Algoritma 'word embedding'?", "id": "Word2Vec, GloVe, FastText." },
  { "en": "Apa itu 'corpus'?", "id": "Kumpulan besar data teks." },
  { "en": "Apa itu 'policy' di RL?", "id": "Strategi agent untuk memilih aksi." },
  { "en": "Apa itu 'Q-learning'?", "id": "Algoritma Reinforcement Learning." },
  { "en": "Beda 'model-based' & 'model-free' RL?", "id": "Model-based punya model lingkungan." },
  { "en": "Apa itu 'explorasi' di RL?", "id": "Mencoba aksi baru secara acak." },
  { "en": "Apa itu 'eksploitasi' di RL?", "id": "Memilih aksi terbaik yang diketahui." },
  { "en": "Apa itu 'data labelling'?", "id": "Proses memberi label pada data." },
  { "en": "Pekerjaan 'data labelling'?", "id": "Data annotator." },
  { "en": "Alat bantu 'data labelling'?", "id": "Labelbox, CVAT." },
  { "en": "Apa itu 'active learning' AI?", "id": "AI memilih data untuk dilabeli." },
  { "en": "Perbedaan 'Narrow AI'?", "id": "AI spesialis untuk satu tugas." },
  { "en": "Perbedaan 'General AI' (AGI)?", "id": "AI secerdas manusia di semua tugas." },
  { "en": "Apakah AGI (Artificial General Intelligence) sudah ada?", "id": "Belum, masih dalam tahap riset." },
  { "en": "Apa itu 'Singularity'?", "id": "Momen AI melampaui kecerdasan manusia." },
  { "en": "Apa itu 'computer graphics'?", "id": "Membuat gambar menggunakan komputer." },
  { "en": "Beda 'computer vision' & 'graphics'?", "id": "Vision memahami, graphics membuat." },
  { "en": "Apa itu '3D reconstruction'?", "id": "Membangun model 3D dari gambar 2D." },
  { "en": "Apa itu 'SLAM (Simultaneous Localization and Mapping)'?", "id": "Memetakan lingkungan sambil melacak posisi." },
  { "en": "Aplikasi SLAM (Simultaneous Localization and Mapping)?", "id": "Robot otonom, augmented reality." },
  { "en": "Apa itu 'optical flow'?", "id": "Pola pergerakan objek antar frame." },
  { "en": "Tujuan 'optical flow'?", "id": "Estimasi kecepatan dan arah gerakan." },
  { "en": "Apa itu 'image stitching'?", "id": "Menggabungkan beberapa gambar jadi panorama." },
  { "en": "Apa itu 'data-driven approach'?", "id": "Pendekatan berbasis data, bukan aturan." },
  { "en": "Tugas AI persepsi?", "id": "Melihat (visi) dan mendengar (audio)." },
  { "en": "Tugas AI kognisi?", "id": "Berpikir, merencanakan, dan belajar." },
  { "en": "Apa itu 'strong AI'?", "id": "Sinonim untuk AGI." },
  { "en": "Apa itu 'weak AI'?", "id": "Sinonim untuk Narrow AI." },
  { "en": "Apa itu 'symbolic AI'?", "id": "AI berbasis aturan dan logika." },
  { "en": "Nama lain 'symbolic AI'?", "id": "Good Old-Fashioned AI (GOFAI)." },
  { "en": "Apa itu 'sub-symbolic AI'?", "id": "AI berbasis jaringan saraf (belajar)." },
  { "en": "Kapan 'expert system' populer?", "id": "Era symbolic AI (1980-an)." },
  { "en": "Apa itu 'knowledge base'?", "id": "Basis pengetahuan dalam expert system." },
  { "en": "Apa itu 'inference engine'?", "id": "Mesin penalar dalam expert system." },
  { "en": "Apa itu 'feature engineering'?", "id": "Proses memilih dan membuat fitur." },
  { "en": "Pentingnya 'feature engineering'?", "id": "Sangat mempengaruhi performa model ML." },
  { "en": "Apa itu 'curse of dimensionality'?", "id": "Masalah pada data berdimensi tinggi." },
  { "en": "Solusi 'curse of dimensionality'?", "id": "Dimensionality reduction (misal: PCA)." },
  { "en": "Apa itu 'linear regression'?", "id": "Model regresi dengan garis lurus." },
  { "en": "Apa itu 'logistic regression'?", "id": "Model untuk tugas klasifikasi biner." },
  { "en": "Apa itu 'decision boundary'?", "id": "Garis pemisah antar kelas." },
  { "en": "Bentuk 'decision boundary' SVM?", "id": "Hyperplane." },
  { "en": "Apa itu 'support vector'?", "id": "Titik data terdekat ke hyperplane." },
  { "en": "Apa itu 'kernel trick'?", "id": "Teknik SVM untuk data non-linear." },
  { "en": "Contoh 'kernel' SVM?", "id": "Linear, Polynomial, RBF." },
  { "en": "Kelemahan KNN (K-Nearest Neighbors)?", "id": "Lambat saat prediksi, butuh banyak memori." },
  { "en": "Apa itu 'lazy learning'?", "id": "Model yang tidak belajar hingga prediksi." },
  { "en": "Contoh algoritma 'lazy learning'?", "id": "K-Nearest Neighbors (KNN)." },
  { "en": "Apa itu 'eager learning'?", "id": "Model yang membangun model saat training." },
  { "en": "Contoh algoritma 'eager learning'?", "id": "Decision Tree, SVM." },
  { "en": "Apa itu 'information gain'?", "id": "Ukuran untuk memilih fitur di decision tree." },
  { "en": "Apa itu 'pruning'?", "id": "Proses memangkas decision tree." },
  { "en": "Tujuan 'pruning'?", "id": "Mencegah overfitting pada decision tree." },
  { "en": "Apa itu 'random forest'?", "id": "Kumpulan dari banyak decision tree." },
  { "en": "Apa itu 'hierarchical clustering'?", "id": "Metode clustering yang membentuk hierarki." },
  { "en": "Apa itu 'association rule mining'?", "id": "Menemukan aturan asosiasi dalam data." },
  { "en": "Contoh 'association rule'?", "id": "Orang beli roti, sering beli selai." },
  { "en": "Algoritma 'association rule'?", "id": "Apriori, Eclat." },
  { "en": "Apa itu 'outlier detection'?", "id": "Mendeteksi data yang menyimpang jauh." },
  { "en": "Sinonim 'outlier'?", "id": "Anomali." },
  { "en": "Struktur dasar perceptron?", "id": "Input, weights, bias, activation function." },
  { "en": "Beda 'single-layer' & 'multi-layer' perceptron?", "id": "Multi-layer punya hidden layer." },
  { "en": "Fungsi 'hidden layer'?", "id": "Mempelajari pola data yang kompleks." },
  { "en": "Apa itu 'feedforward network'?", "id": "Jaringan dengan aliran informasi maju." },
  { "en": "Apa itu 'gradient descent'?", "id": "Algoritma optimisasi untuk minimalkan loss." },
  { "en": "Apa itu 'vanishing gradient problem'?", "id": "Masalah gradien sangat kecil di RNN." },
  { "en": "Apa itu 'exploding gradient problem'?", "id": "Masalah gradien sangat besar di RNN." },
  { "en": "Solusi 'vanishing gradient'?", "id": "Menggunakan LSTM atau GRU." },
  { "en": "Apa itu 'autoencoder'?", "id": "Neural network untuk kompresi data." },
  { "en": "Struktur 'autoencoder'?", "id": "Encoder dan Decoder." },
  { "en": "Fungsi 'encoder'?", "id": "Mengompres data ke representasi kecil." },
  { "en": "Fungsi 'decoder'?", "id": "Merekonstruksi data dari kompresi." },
  { "en": "Aplikasi 'autoencoder'?", "id": "Denoising, anomaly detection." },
  { "en": "Apa itu 'semantic gap'?", "id": "Kesenjangan antara fitur level rendah-konsep." },
  { "en": "Filter 'Gabor' untuk apa?", "id": "Ekstraksi tekstur pada gambar." },
  { "en": "Apa itu 'Haar-like features'?", "id": "Fitur untuk deteksi objek cepat." },
  { "en": "Penggunaan 'Haar-like features'?", "id": "Deteksi wajah (algoritma Viola-Jones)." },
  { "en": "Apa itu 'affine transformation'?", "id": "Transformasi geometri (rotasi, skala)." },
  { "en": "Apa itu 'perspective transformation'?", "id": "Transformasi untuk memperbaiki sudut pandang." },
  { "en": "Apa itu 'image registration'?", "id": "Menyelaraskan beberapa gambar jadi satu." },
  { "en": "Apa itu 'image stitching'?", "id": "Menggabungkan gambar jadi panorama." },
  { "en": "Tugas 'object tracking'?", "id": "Melacak posisi objek di video." },
  { "en": "Algoritma 'object tracking'?", "id": "Kalman Filter, Particle Filter." },
  { "en": "Apa itu 'scene understanding'?", "id": "Memahami konteks keseluruhan sebuah gambar." },
  { "en": "Apa itu 'action recognition'?", "id": "Mengenali aksi manusia dalam video." },
  { "en": "Tantangan 'action recognition'?", "id": "Variasi kecepatan, sudut pandang." },
  { "en": "Apa itu 'gaze estimation'?", "id": "Memperkirakan arah pandangan mata." },
  { "en": "Apa itu 'fine-tuning' model?", "id": "Melatih ulang layer terakhir model." },
  { "en": "Kapan 'fine-tuning' digunakan?", "id": "Saat dataset baru mirip dataset asli." },
  { "en": "Beda 'fine-tuning' & 'transfer learning'?", "id": "Fine-tuning adalah salah satu cara." },
  { "en": "Arsitektur 'encoder-decoder'?", "id": "Struktur umum untuk banyak tugas AI." },
  { "en": "Contoh aplikasi 'encoder-decoder'?", "id": "Machine translation, image captioning." },
  { "en": "Apa itu 'image captioning'?", "id": "Membuat deskripsi teks untuk gambar." },
  { "en": "Apa itu 'visual question answering' (VQA)?", "id": "Menjawab pertanyaan tentang sebuah gambar." },
  { "en": "Apa itu 'discriminator' di GAN?", "id": "Jaringan yang menilai keaslian gambar." },
  { "en": "Apa itu 'generator' di GAN?", "id": "Jaringan yang membuat gambar palsu." },
  { "en": "Tujuan training GAN (Generative Adversarial Network)?", "id": "Generator menipu diskriminator." },
  { "en": "Apa itu 'mode collapse'?", "id": "Masalah pada training GAN." },
  { "en": "Variasi GAN (Generative Adversarial Network)?", "id": "CycleGAN, StyleGAN." },
  { "en": "Apa itu 'landmark detection'?", "id": "Mendeteksi titik kunci pada objek." },
  { "en": "Contoh 'landmark detection' wajah?", "id": "Posisi mata, hidung, dan mulut." },
  { "en": "Apa itu 'biometric spoofing'?", "id": "Memalsukan ciri biometrik." },
  { "en": "Contoh 'biometric spoofing'?", "id": "Pakai foto untuk kelabui face ID." },
  { "en": "Apa itu 'liveness detection'?", "id": "Memastikan biometrik berasal dari orang hidup." },
  { "en": "Apa itu 'cross-validation'?", "id": "Teknik evaluasi model lebih robust." },
  { "en": "Metode 'k-fold cross-validation'?", "id": "Data dibagi menjadi k bagian." },
  { "en": "Apa itu 'data leakage'?", "id": "Bocornya informasi data tes ke training." },
  { "en": "Dampak 'data leakage'?", "id": "Evaluasi model menjadi terlalu optimis." },
  { "en": "Apa itu 'black box AI'?", "id": "Model AI yang sulit dijelaskan." },
  { "en": "Apa itu 'white box AI'?", "id": "Model AI yang mudah diinterpretasi." },
  { "en": "Contoh model 'white box'?", "id": "Linear Regression, Decision Tree." },
  { "en": "Teknik untuk XAI (Explainable AI)?", "id": "LIME, SHAP." },
  { "en": "Kepanjangan LIME (Local Interpretable Model-agnostic Explanations)?", "id": "Local Interpretable Model-agnostic Explanations." },
  { "en": "Apa itu 'adversarial robustness'?", "id": "Ketahanan model terhadap serangan adversarial." },
  { "en": "Apa itu 'edge device'?", "id": "Perangkat komputasi di tepi jaringan." },
  { "en": "Contoh 'edge device'?", "id": "Smartphone, kamera pintar, Raspberry Pi." },
  { "en": "Apa itu 'edge AI'?", "id": "Menjalankan inferensi AI di edge device." },
  { "en": "Keuntungan 'edge AI'?", "id": "Latensi rendah, privasi, hemat bandwidth." },
  { "en": "Apa itu 'point cloud'?", "id": "Kumpulan titik data dalam ruang 3D." },
  { "en": "Sumber data 'point cloud'?", "id": "Sensor LiDAR, kamera kedalaman." },
  { "en": "Apa itu arsitektur VGGNet?", "id": "CNN dalam dengan filter 3x3." },
  { "en": "Apa itu arsitektur ResNet?", "id": "CNN dengan 'residual connections'." },
  { "en": "Fungsi 'residual connection'?", "id": "Mengatasi 'vanishing gradient problem'." },
  { "en": "Apa itu arsitektur Inception (GoogLeNet)?", "id": "Menggunakan 'inception module' paralel." },
  { "en": "Apa itu arsitektur MobileNet?", "id": "CNN efisien untuk perangkat mobile." },
  { "en": "Teknik yang digunakan MobileNet?", "id": "Depthwise separable convolutions." },
  { "en": "Detektor 'two-stage'?", "id": "Proposal wilayah lalu klasifikasi." },
  { "en": "Contoh detektor 'two-stage'?", "id": "R-CNN, Fast R-CNN, Faster R-CNN." },
  { "en": "Detektor 'one-stage'?", "id": "Langsung memprediksi 'bounding box'." },
  { "en": "Contoh detektor 'one-stage'?", "id": "YOLO, SSD." },
  { "en": "Beda utama 'one-stage' & 'two-stage'?", "id": "One-stage lebih cepat, two-stage akurat." },
  { "en": "Kepanjangan R-CNN (Region-based CNN)?", "id": "Region-based Convolutional Neural Network." },
  { "en": "Kepanjangan SSD (Single Shot Detector)?", "id": "Single Shot MultiBox Detector." },
  { "en": "Apa itu 'anchor boxes'?", "id": "Kotak referensi pada object detection." },
  { "en": "Apa itu 'IoU (Intersection over Union)'?", "id": "Metrik akurasi untuk 'bounding box'." },
  { "en": "Fungsi 'Non-Maximum Suppression' (NMS)?", "id": "Menghapus 'bounding box' tumpang tindih." },
  { "en": "Output 'panoptic segmentation'?", "id": "Kombinasi semantic dan instance." },
  { "en": "Arsitektur populer untuk segmentasi?", "id": "U-Net, Mask R-CNN." },
  { "en": "Ciri khas arsitektur U-Net?", "id": "Bentuk seperti huruf U." },
  { "en": "Fungsi 'skip connection' di U-Net?", "id": "Menggabungkan fitur resolusi tinggi-rendah." },
  { "en": "Apa itu 'pose' manusia?", "id": "Orientasi dan konfigurasi bagian tubuh." },
  { "en": "Output '2D pose estimation'?", "id": "Koordinat (x,y) sendi tubuh." },
  { "en": "Output '3D pose estimation'?", "id": "Koordinat (x,y,z) sendi tubuh." },
  { "en": "Apa itu 'style transfer'?", "id": "Mengaplikasikan gaya satu gambar ke lain." },
  { "en": "Apa itu 'super-resolution'?", "id": "Meningkatkan resolusi gambar dengan AI." },
  { "en": "Variasi GAN (Generative Adversarial Network) untuk citra?", "id": "StyleGAN, CycleGAN." },
  { "en": "Fungsi 'CycleGAN'?", "id": "Translasi gambar tanpa pasangan data." },
  { "en": "Apa itu VAE (Variational Autoencoder)?", "id": "Model generatif berbasis probabilitas." },
  { "en": "Apa itu 'diffusion model'?", "id": "Model generatif dengan proses noising-denoising." },
  { "en": "Kelebihan 'diffusion model'?", "id": "Menghasilkan gambar berkualitas sangat tinggi." },
  { "en": "Apa itu 'data imbalance'?", "id": "Jumlah data tidak seimbang antar kelas." },
  { "en": "Dampak 'data imbalance'?", "id": "Model cenderung memihak kelas mayoritas." },
  { "en": "Solusi 'data imbalance'?", "id": "Oversampling, undersampling, SMOTE." },
  { "en": "Apa itu 'oversampling'?", "id": "Duplikasi data pada kelas minoritas." },
  { "en": "Apa itu 'undersampling'?", "id": "Mengurangi data pada kelas mayoritas." },
  { "en": "Kepanjangan SMOTE?", "id": "Synthetic Minority Over-sampling Technique." },
  { "en": "Apa itu 'generalisasi' model?", "id": "Kemampuan model bekerja di data baru." },
  { "en": "Apa itu 'inductive bias'?", "id": "Asumsi yang dibuat model." },
  { "en": "Apa itu 'Bayesian deep learning'?", "id": "Deep learning yang mengukur ketidakpastian." },
  { "en": "Apa itu 'gradient'?", "id": "Arah perubahan 'loss' tercepat." },
  { "en": "Apa itu 'saddle point'?", "id": "Titik datar yang bukan minimum." },
  { "en": "Apa itu 'local minima'?", "id": "Titik terendah di area lokal." },
  { "en": "Apa itu 'global minima'?", "id": "Titik terendah di seluruh area." },
  { "en": "Apa itu 'convex optimization'?", "id": "Optimisasi dengan satu solusi global." },
  { "en": "Apakah deep learning 'convex'?", "id": "Tidak, bersifat non-convex." },
  { "en": "Apa itu 'batch normalization'?", "id": "Normalisasi input di setiap layer." },
  { "en": "Tujuan 'batch normalization'?", "id": "Mempercepat training, menstabilkan jaringan." },
  { "en": "Beda 'self-supervised learning'?", "id": "Label dibuat otomatis dari data." },
  { "en": "Apa itu 'contrastive learning'?", "id": "Jenis 'self-supervised learning'." },
  { "en": "Apa itu 'few-shot learning'?", "id": "Belajar efektif dari sedikit contoh." },
  { "en": "Apa itu 'meta-learning'?", "id": "Belajar cara untuk belajar." },
  { "en": "Sinonim 'meta-learning'?", "id": "Learning to learn." },
  { "en": "Apa itu 'image retrieval'?", "id": "Mencari gambar mirip dari database." },
  { "en": "Contoh 'image retrieval'?", "id": "Google Image Search." },
  { "en": "Apa itu 'semantic search'?", "id": "Pencarian berdasarkan makna, bukan kata kunci." },
  { "en": "Apa itu 'graph neural network' (GNN)?", "id": "Jaringan saraf untuk data graf." },
  { "en": "Contoh data 'graph'?", "id": "Jaringan sosial, molekul kimia." },
  { "en": "Kepanjangan GNN (Graph Neural Network)?", "id": "Graph Neural Network." },
  { "en": "Apa itu 'attention heatmap'?", "id": "Visualisasi area fokus model AI." },
  { "en": "Apa itu 'gradient-weighted class activation mapping' (Grad-CAM)?", "id": "Teknik visualisasi untuk XAI." },
  { "en": "Apa itu 'data augmentation'?", "id": "Memperbanyak data latih secara sintetis." },
  { "en": "Contoh 'data augmentation' gambar?", "id": "Rotasi, flip, zoom, ubah warna." },
  { "en": "Apa itu 'test-time augmentation' (TTA)?", "id": "Augmentasi saat melakukan inferensi/prediksi." },
  { "en": "Apa itu 'model quantization'?", "id": "Mengurangi presisi 'weights' model." },
  { "en": "Tujuan 'model quantization'?", "id": "Memperkecil model, percepat inferensi." },
  { "en": "Apa itu 'model pruning'?", "id": "Menghapus koneksi/neuron yang tidak penting." },
  { "en": "Apa itu 'knowledge distillation'?", "id": "Model kecil belajar dari model besar." },
  { "en": "Model besar disebut?", "id": "Teacher model." },
  { "en": "Model kecil disebut?", "id": "Student model." },
  { "en": "Apa itu 'multitask learning'?", "id": "Satu model belajar banyak tugas." },
  { "en": "Keuntungan 'multitask learning'?", "id": "Efisiensi dan bisa tingkatkan performa." },
  { "en": "Apa itu 'hard parameter sharing'?", "id": "Berbagi layer bawah, beda output." },
  { "en": "Apa itu 'soft parameter sharing'?", "id": "Setiap tugas punya model sendiri." },
  { "en": "Peran 'dataset' dalam AI?", "id": "Bahan bakar untuk melatih model." },
  { "en": "Dataset gambar terkenal?", "id": "ImageNet, COCO, MNIST." },
  { "en": "Isi dataset MNIST?", "id": "Gambar tulisan tangan angka." },
  { "en": "Isi dataset COCO?", "id": "Gambar dengan anotasi deteksi/segmentasi." },
  { "en": "Apa itu 'data annotation'?", "id": "Proses memberi label pada data." },
  { "en": "Contoh 'data annotation'?", "id": "Menggambar 'bounding box' pada mobil." },
  { "en": "Apa itu 'data pipeline'?", "id": "Alur kerja dari data mentah-model." },
  { "en": "Tahap dalam 'data pipeline'?", "id": "Ingestion, preprocessing, training, deployment." },
  { "en": "Apa itu 'model deployment'?", "id": "Proses menempatkan model di produksi." },
  { "en": "Apa itu 'API (Application Programming Interface)' model?", "id": "Antarmuka untuk akses model AI." },
  { "en": "Apa itu 'latency' model?", "id": "Waktu yang dibutuhkan untuk satu prediksi." },
  { "en": "Apa itu 'throughput' model?", "id": "Jumlah prediksi per satuan waktu." },
  { "en": "Apa itu 'AutoML'?", "id": "Otomatisasi proses pengembangan model ML." },
  { "en": "Tujuan 'AutoML'?", "id": "Membuat AI lebih mudah diakses." },
  { "en": "Apa itu 'human-in-the-loop' (HITL)?", "id": "Melibatkan manusia dalam siklus AI." },
  { "en": "Contoh HITL (Human-in-the-loop)?", "id": "Manusia verifikasi prediksi AI." },
  { "en": "Apa itu 'data validation'?", "id": "Memastikan kualitas dan format data." },
  { "en": "Apa itu 'feature store'?", "id": "Penyimpanan terpusat untuk fitur ML." },
  { "en": "Apa itu 'model registry'?", "id": "Penyimpanan terpusat untuk versi model." },
  { "en": "Apa itu 'A/B testing' model?", "id": "Membandingkan performa dua model." },
  { "en": "Apa itu 'shadow deployment'?", "id": "Menjalankan model baru tanpa pengaruhi user." },
  { "en": "Apa itu 'canary deployment'?", "id": "Rilis model baru ke sebagian kecil user." },
  { "en": "Apa itu 'model monitoring'?", "id": "Memantau performa model di produksi." },
  { "en": "Apa itu 'gradient checking'?", "id": "Proses verifikasi perhitungan backpropagation." },
  { "en": "Apa itu 'numerical stability'?", "id": "Ketahanan algoritma terhadap error kecil." },
  { "en": "Apa itu 'internal covariate shift'?", "id": "Perubahan distribusi input setiap layer." },
  { "en": "Solusi 'internal covariate shift'?", "id": "Batch Normalization." },
  { "en": "Apa itu 'Glorot/Xavier initialization'?", "id": "Teknik inisialisasi 'weights' yang populer." },
  { "en": "Apa itu 'He initialization'?", "id": "Inisialisasi 'weights' khusus untuk ReLU." },
  { "en": "Fungsi 'softmax'?", "id": "Mengubah output menjadi distribusi probabilitas." },
  { "en": "Di mana 'softmax' digunakan?", "id": "Layer output pada tugas klasifikasi." },
  { "en": "Apa itu 'one-hot encoding'?", "id": "Representasi data kategori sebagai vektor." },
  { "en": "Contoh 'one-hot encoding'?", "id": "Kelas 'kucing' menjadi [1, 0, 0]." },
  { "en": "Apa itu 'cosine similarity'?", "id": "Mengukur kemiripan arah dua vektor." },
  { "en": "Rentang nilai 'cosine similarity'?", "id": "Dari -1 hingga 1." },
  { "en": "Nilai 1 'cosine similarity' artinya?", "id": "Vektor menunjuk ke arah sama." },
  { "en": "Apa itu 'Euclidean distance'?", "id": "Jarak garis lurus antara dua titik." },
  { "en": "Apa itu 'Manhattan distance'?", "id": "Jarak dengan jalur seperti blok kota." },
  { "en": "Apa itu 'manifold learning'?", "id": "Teknik 'dimensionality reduction' non-linear." },
  { "en": "Contoh algoritma 'manifold learning'?", "id": "t-SNE, UMAP." },
  { "en": "Kepanjangan t-SNE?", "id": "t-Distributed Stochastic Neighbor Embedding." },
  { "en": "Tujuan utama t-SNE?", "id": "Visualisasi data berdimensi tinggi." },
  { "en": "Apa itu 'bag-of-words' (BoW)?", "id": "Representasi teks dari frekuensi kata." },
  { "en": "Kelemahan 'bag-of-words'?", "id": "Mengabaikan urutan dan makna kata." },
  { "en": "Apa itu 'TF-IDF'?", "id": "Pembobotan kata berdasarkan frekuensi dan keunikan." },
  { "en": "Kepanjangan TF-IDF?", "id": "Term Frequency-Inverse Document Frequency." },
  { "en": "Apa itu 'topic modeling'?", "id": "Menemukan topik tersembunyi dalam teks." },
  { "en": "Algoritma 'topic modeling'?", "id": "LDA (Latent Dirichlet Allocation)." },
  { "en": "Apa itu 'sentiment analysis'?", "id": "Menganalisa sentimen (positif/negatif/netral)." },
  { "en": "Aplikasi 'sentiment analysis'?", "id": "Analisa opini publik di media sosial." },
  { "en": "Apa itu 'named-entity recognition' (NER)?", "id": "Mengenali entitas (nama, lokasi) di teks." },
  { "en": "Kepanjangan NER?", "id": "Named-Entity Recognition." },
  { "en": "Apa itu 'part-of-speech' (POS) tagging?", "id": "Memberi label kelas kata (kata benda, kerja)." },
  { "en": "Apa itu 'language model'?", "id": "Model probabilitas urutan kata." },
  { "en": "Apa itu 'BLEU score'?", "id": "Metrik evaluasi untuk 'machine translation'." },
  { "en": "Apa itu 'perceptual loss'?", "id": "Loss function berbasis fitur persepsi." },
  { "en": "Penggunaan 'perceptual loss'?", "id": "Tugas generatif seperti 'style transfer'." },
  { "en": "Apa itu 'receptive field'?", "id": "Area input yang dilihat satu neuron." },
  { "en": "Apa itu '1x1 convolution'?", "id": "Konvolusi untuk ubah jumlah channel." },
  { "en": "Apa itu 'dilated convolution'?", "id": "Konvolusi dengan 'receptive field' lebih luas." },
  { "en": "Apa itu 'transposed convolution'?", "id": "Operasi untuk 'upsampling' fitur map." },
  { "en": "Sinonim 'transposed convolution'?", "id": "Deconvolution." },
  { "en": "Apa itu 'global average pooling' (GAP)?", "id": "Mengubah 'feature map' menjadi satu nilai." },
  { "en": "Apa itu 'data augmentation' offline?", "id": "Augmentasi dilakukan sebelum training." },
  { "en": "Apa itu 'data augmentation' online?", "id": "Augmentasi dilakukan saat training." },
  { "en": "Tantangan pada 'face verification'?", "id": "Memverifikasi apakah dua wajah sama." },
  { "en": "Tantangan pada 'face identification'?", "id": "Mencari wajah di database." },
  { "en": "Apa itu 'Siamese network'?", "id": "Jaringan untuk mengukur kemiripan input." },
  { "en": "Aplikasi 'Siamese network'?", "id": "Face verification, signature verification." },
  { "en": "Apa itu 'triplet loss'?", "id": "Loss function untuk belajar kemiripan." },
  { "en": "Komponen 'triplet loss'?", "id": "Anchor, positive, dan negative." },
  { "en": "Apa itu 'optical music recognition' (OMR)?", "id": "Membaca notasi musik dari gambar." },
  { "en": "Apa itu 'generative query network' (GQN)?", "id": "AI yang bisa membayangkan objek 3D." },
  { "en": "Apa itu 'world model'?", "id": "Model internal lingkungan pada agent." },
  { "en": "Apa itu 'Monte Carlo Tree Search' (MCTS)?", "id": "Algoritma pencarian untuk game." },
  { "en": "AlphaGo menggunakan algoritma apa?", "id": "Deep Learning dan MCTS." },
  { "en": "Apa itu 'Markov Decision Process' (MDP)?", "id": "Kerangka matematis untuk Reinforcement Learning." },
  { "en": "Elemen MDP (Markov Decision Process)?", "id": "States, actions, rewards, transitions." },
  { "en": "Apa itu 'state' dalam RL?", "id": "Representasi lingkungan saat ini." },
  { "en": "Apa itu 'action' dalam RL?", "id": "Pilihan yang bisa diambil agent." },
  { "en": "Apa itu 'discount factor' (gamma)?", "id": "Parameter pentingnya reward masa depan." },
  { "en": "Apa itu 'value function'?", "id": "Memprediksi total reward masa depan." },
  { "en": "Beda 'on-policy' & 'off-policy' RL?", "id": "On-policy belajar dari aksi sendiri." },
  { "en": "Contoh algoritma 'on-policy'?", "id": "SARSA." },
  { "en": "Contoh algoritma 'off-policy'?", "id": "Q-Learning." },
  { "en": "Apa itu 'Deep Q-Network' (DQN)?", "id": "Q-learning menggunakan Deep Learning." },
  { "en": "Apa itu 'policy gradient' methods?", "id": "Langsung mengoptimalkan 'policy' agent." },
  { "en": "Apa itu 'Actor-Critic' methods?", "id": "Menggabungkan 'value-based' dan 'policy-based'." },
  { "en": "Komponen 'Actor-Critic'?", "id": "Actor (policy) dan Critic (value)." },
  { "en": "Apa itu 'curriculum learning'?", "id": "Melatih model dari mudah ke sulit." },
  { "en": "Apa itu 'AI safety'?", "id": "Riset untuk membuat AI aman." },
  { "en": "Apa itu 'AI alignment'?", "id": "Memastikan tujuan AI selaras manusia." },
  { "en": "Apa itu 'robustness' AI?", "id": "Ketahanan model terhadap gangguan." },
  { "en": "Apa itu 'fairness' AI?", "id": "Memastikan model tidak diskriminatif." },
  { "en": "Apa itu 'transparency' AI?", "id": "Keterbukaan cara kerja model." },
  { "en": "Apa itu 'privacy-preserving AI'?", "id": "AI yang melindungi privasi data." },
  { "en": "Apa itu 'differential privacy'?", "id": "Teknik melindungi privasi dalam dataset." },
  { "en": "Apa itu 'homomorphic encryption'?", "id": "Enkripsi yang bisa diolah datanya." },
  { "en": "Apa itu 'Capsule Network'?", "id": "Alternatif dari arsitektur CNN." },
  { "en": "Keuntungan 'Capsule Network'?", "id": "Lebih baik memahami hubungan spasial." },
  { "en": "Apa itu 'attention' di Transformer?", "id": "Mekanisme pembobotan relevansi input." },
  { "en": "Apa itu 'self-attention'?", "id": "Attention antar elemen dalam satu sekuens." },
  { "en": "Apa itu 'positional encoding'?", "id": "Menambahkan informasi posisi ke input." },
  { "en": "Kenapa Transformer butuh 'positional encoding'?", "id": "Karena tidak memproses data berurutan." },
  { "en": "Apa itu 'Vision Transformer' (ViT)?", "id": "Model Transformer untuk tugas visi." },
  { "en": "Apa itu 'spiking neural network' (SNN)?", "id": "Jaringan saraf terinspirasi otak biologis." },
  { "en": "Kelebihan SNN (Spiking Neural Network)?", "id": "Sangat efisien dalam penggunaan daya." },
  { "en": "Apa itu 'neuromorphic computing'?", "id": "Komputasi yang meniru arsitektur otak." },
  { "en": "Apa itu 'gradient checking'?", "id": "Proses verifikasi perhitungan 'backpropagation'." },
  { "en": "Apa itu 'numerical stability'?", "id": "Ketahanan algoritma terhadap eror pembulatan." },
  { "en": "Apa itu 'activation saturation'?", "id": "Kondisi output aktivasi tidak berubah." },
  { "en": "Aktivasi yang rentan saturasi?", "id": "Sigmoid dan Tanh." },
  { "en": "Aktivasi yang tidak saturasi?", "id": "ReLU dan variasinya." },
  { "en": "Apa itu 'Bayesian optimization'?", "id": "Metode efisien untuk 'hyperparameter tuning'." },
  { "en": "Apa itu 'grid search'?", "id": "Mencoba semua kombinasi hyperparameter." },
  { "en": "Apa itu 'random search'?", "id": "Mencoba kombinasi hyperparameter acak." },
  { "en": "Mana lebih efisien, 'grid'/'random search'?", "id": "Random search seringkali lebih efisien." },
  { "en": "Apa itu 'human baseline'?", "id": "Performa manusia sebagai tolok ukur." },
  { "en": "Apa itu 'model versioning'?", "id": "Melacak dan mengelola versi model." },
  { "en": "Pentingnya 'model versioning'?", "id": "Memastikan reproduktifitas dan rollback." },
  { "en": "Apa itu 'CI/CD for ML'?", "id": "Otomatisasi siklus hidup model ML." },
  { "en": "Kepanjangan CI/CD?", "id": "Continuous Integration/Continuous Deployment." },
  { "en": "Metrik untuk 'fairness' AI?", "id": "Demographic parity, equal opportunity." },
  { "en": "Apa itu 'demographic parity'?", "id": "Hasil prediksi tidak bergantung grup." },
  { "en": "Apa itu 'accountability' AI?", "id": "Prinsip pertanggungjawaban atas sistem AI." },
  { "en": "Arsitektur GPU (Graphics Processing Unit) untuk AI?", "id": "Banyak core untuk komputasi paralel." },
  { "en": "Unit komputasi di GPU Nvidia?", "id": "CUDA Cores, Tensor Cores." },
  { "en": "Fungsi 'Tensor Cores'?", "id": "Mempercepat operasi matriks di deep learning." },
  { "en": "Pengembang TPU (Tensor Processing Unit)?", "id": "Google." },
  { "en": "Apa itu 'neuromorphic chip'?", "id": "Chip yang meniru struktur otak." },
  { "en": "Apa itu 'data-centric AI'?", "id": "Fokus pada peningkatan kualitas data." },
  { "en": "Lawan dari 'data-centric AI'?", "id": "Model-centric AI." },
  { "en": "Apa itu 'video understanding'?", "id": "AI memahami konten dan konteks video." },
  { "en": "Tantangan 'video understanding'?", "id": "Kompleksitas temporal, volume data besar." },
  { "en": "Apa itu 'action localization'?", "id": "Deteksi aksi dan lokasinya di video." },
  { "en": "Apa itu 'dense captioning'?", "id": "Membuat deskripsi untuk setiap event video." },
  { "en": "Input untuk '3D computer vision'?", "id": "Stereo images, point clouds, video." },
  { "en": "Apa itu 'SLAM (Simultaneous Localization and Mapping)'?", "id": "Memetakan lingkungan sambil melacak posisi." },
  { "en": "Aplikasi SLAM (Simultaneous Localization and Mapping)?", "id": "Robot otonom, augmented reality." },
  { "en": "Apa itu 'structure from motion' (SfM)?", "id": "Membangun adegan 3D dari gambar 2D." },
  { "en": "Kepanjangan SfM?", "id": "Structure from Motion." },
  { "en": "Beda SLAM (Simultaneous Localization and Mapping) dan SfM (Structure from Motion)?", "id": "SLAM real-time, SfM offline." },
  { "en": "Apa itu 'multi-view stereo' (MVS)?", "id": "Membangun 3D dari banyak gambar." },
  { "en": "Apa itu 'generative adversarial imitation learning' (GAIL)?", "id": "Belajar perilaku dari contoh (demonstrasi)." },
  { "en": "Apa itu 'inverse reinforcement learning' (IRL)?", "id": "Menemukan 'reward function' dari observasi." },
  { "en": "Apa itu 'Transformer' dalam AI?", "id": "Arsitektur model berbasis 'self-attention'." },
  { "en": "Awalnya 'Transformer' untuk tugas apa?", "id": "Tugas NLP (Natural Language Processing) seperti translasi." },
  { "en": "Apa itu 'Vision Transformer' (ViT)?", "id": "Model Transformer untuk tugas visi." },
  { "en": "Bagaimana ViT (Vision Transformer) proses gambar?", "id": "Memecah gambar menjadi 'patches'." },
  { "en": "Apa itu 'token' dalam AI?", "id": "Unit dasar input (kata/patch gambar)." },
  { "en": "Apa itu 'causal attention'?", "id": "Attention yang hanya lihat masa lalu." },
  { "en": "Penggunaan 'causal attention'?", "id": "Model bahasa generatif (misal: GPT)." },
  { "en": "Apa itu 'Bayesian neural network'?", "id": "Neural network dengan bobot probabilistik." },
  { "en": "Keuntungan 'Bayesian neural network'?", "id": "Bisa mengukur ketidakpastian prediksi." },
  { "en": "Apa itu 'Gaussian process'?", "id": "Model probabilistik untuk regresi." },
  { "en": "Apa itu 'anomaly detection'?", "id": "Mendeteksi data atau kejadian anomali." },
  { "en": "Aplikasi 'anomaly detection'?", "id": "Deteksi fraud, monitoring kesehatan sistem." },
  { "en": "Apa itu 'weakly supervised learning'?", "id": "Belajar dari label yang tidak presisi." },
  { "en": "Contoh 'weak supervision'?", "id": "Label level gambar untuk segmentasi." },
  { "en": "Apa itu 'semi-supervised learning'?", "id": "Belajar dari data berlabel sedikit." },
  { "en": "Apa itu 'active learning'?", "id": "Model aktif meminta label." },
  { "en": "Tujuan 'active learning'?", "id": "Mengurangi jumlah data yang perlu dilabeli." },
  { "en": "Apa itu 'causal inference'?", "id": "Menyimpulkan hubungan sebab-akibat dari data." },
  { "en": "Beda korelasi dan kausalitas?", "id": "Korelasi tidak berarti sebab-akibat." },
  { "en": "Apa itu 'counterfactual'?", "id": "Memikirkan 'apa yang terjadi jika'." },
  { "en": "Apa itu 'data flywheel'?", "id": "Siklus dimana produk-data-model saling memperbaiki." },
  { "en": "Apa itu 'human-computer interaction' (HCI)?", "id": "Studi interaksi manusia dan komputer." },
  { "en": "Kepanjangan HCI?", "id": "Human-Computer Interaction." },
  { "en": "Kaitan HCI (Human-Computer Interaction) dan AI?", "id": "Desain antarmuka AI yang intuitif." },
  { "en": "Apa itu 'convolution'?", "id": "Operasi matematis untuk filtering." },
  { "en": "Apa itu 'cross-correlation'?", "id": "Mengukur kemiripan dua sinyal." },
  { "en": "Apa itu 'Fourier transform'?", "id": "Mengubah sinyal ke domain frekuensi." },
  { "en": "Apa itu 'image sharpening'?", "id": "Proses mempertajam detail gambar." },
  { "en": "Filter untuk 'image sharpening'?", "id": "High-pass filter." },
  { "en": "Apa itu 'image blurring'?", "id": "Proses menghaluskan atau mengaburkan gambar." },
  { "en": "Filter untuk 'image blurring'?", "id": "Low-pass filter." },
  { "en": "Contoh 'low-pass filter'?", "id": "Gaussian blur, box blur." },
  { "en": "Apa itu 'median filter'?", "id": "Filter non-linear untuk hapus noise." },
  { "en": "Kelebihan 'median filter'?", "id": "Efektif hapus 'salt-and-pepper noise'." },
  { "en": "Apa itu 'bilateral filter'?", "id": "Filter yang menjaga ketajaman tepi." },
  { "en": "Apa itu 'affine transformation'?", "id": "Transformasi linear (skala, rotasi, geser)." },
  { "en": "Apa itu 'homography'?", "id": "Transformasi antara dua bidang datar." },
  { "en": "Apa itu 'epipolar geometry'?", "id": "Geometri dari sistem dua kamera." },
  { "en": "Apa itu 'stereo vision'?", "id": "Membuat persepsi kedalaman dari dua kamera." },
  { "en": "Apa itu 'disparity map'?", "id": "Peta perbedaan posisi di stereo vision." },
  { "en": "Apa itu 'GAN-based image restoration'?", "id": "Memperbaiki gambar rusak menggunakan GAN." },
  { "en": "Apa itu 'zero-shot image generation'?", "id": "Membuat gambar dari deskripsi teks." },
  { "en": "Contoh model 'text-to-image'?", "id": "DALL-E, Midjourney, Stable Diffusion." },
  { "en": "Apa itu 'prompt engineering'?", "id": "Seni merancang input teks untuk AI." },
  { "en": "Apa itu 'in-context learning'?", "id": "Kemampuan model belajar dari contoh." },
  { "en": "Apa itu 'chain-of-thought' prompting?", "id": "Mendorong AI berpikir langkah-demi-langkah." },
  { "en": "Apa itu 'large language model' (LLM)?", "id": "Model bahasa dengan parameter sangat banyak." },
  { "en": "Kepanjangan LLM?", "id": "Large Language Model." },
  { "en": "Apa itu 'foundation model'?", "id": "Model besar yang bisa diadaptasi." },
  { "en": "Apa itu 'AI alignment problem'?", "id": "Masalah menyelaraskan tujuan AI-manusia." },
  { "en": "Apa itu 'catastrophic forgetting'?", "id": "Model lupa tugas lama." },
  { "en": "Apa itu 'continual learning'?", "id": "Belajar terus-menerus tanpa lupa." },
  { "en": "Apa itu 'spurious correlation'?", "id": "Korelasi palsu yang tidak kausal." },
  { "en": "Apa itu 'adversarial training'?", "id": "Melatih model dengan contoh adversarial." },
  { "en": "Tujuan 'adversarial training'?", "id": "Meningkatkan ketahanan model (robustness)." },
  { "en": "Apa itu 'data augmentation' di NLP?", "id": "Sinonim, parafrase, back-translation." },
  { "en": "Apa itu 'back-translation'?", "id": "Translasi teks bolak-balik." },
  { "en": "Apa itu 'gradient clipping'?", "id": "Mencegah 'exploding gradient problem'." },
  { "en": "Apa itu 'weight decay'?", "id": "Teknik regularisasi L2." },
  { "en": "Apa itu 'early stopping'?", "id": "Menghentikan training saat performa menurun." },
  { "en": "Apa itu 'learning rate schedule'?", "id": "Mengubah learning rate selama training." },
  { "en": "Apa itu 'momentum' optimizer?", "id": "Membantu optimisasi keluar dari local minima." },
  { "en": "Apa itu 'Adam' optimizer?", "id": "Optimizer adaptif yang populer." },
  { "en": "Apa itu 'reproducibility' dalam AI?", "id": "Kemampuan mereproduksi hasil riset." },
  { "en": "Apa itu 'stochasticity'?", "id": "Adanya elemen keacakan." },
  { "en": "Sumber 'stochasticity' dalam training?", "id": "Inisialisasi acak, 'shuffling' data." },
  { "en": "Apa itu 'greedy algorithm'?", "id": "Algoritma yang ambil pilihan terbaik lokal." },
  { "en": "Apakah 'gradient descent' 'greedy'?", "id": "Ya, pada setiap langkahnya." },
  { "en": "Apa itu 'feature scaling'?", "id": "Menyamakan rentang nilai fitur." },
  { "en": "Kenapa 'feature scaling' penting?", "id": "Bantu algoritma konvergen lebih cepat." },
  { "en": "Apa itu 'bias-variance tradeoff'?", "id": "Keseimbangan antara error bias dan varians." },
  { "en": "Model 'high bias'?", "id": "Model terlalu sederhana (underfitting)." },
  { "en": "Model 'high variance'?", "id": "Model terlalu kompleks (overfitting)." },
  { "en": "Tujuan 'cross-validation'?", "id": "Estimasi performa model lebih akurat." },
  { "en": "Apa itu 'leave-one-out cross-validation' (LOOCV)?", "id": "Variasi k-fold dengan k=n." },
  { "en": "Apa itu 'bootstrap aggregating' (bagging)?", "id": "Teknik ensemble dengan training paralel." },
  { "en": "Contoh algoritma 'bagging'?", "id": "Random Forest." },
  { "en": "Apa itu 'boosting'?", "id": "Teknik ensemble dengan training sekuensial." },
  { "en": "Contoh algoritma 'boosting'?", "id": "AdaBoost, Gradient Boosting, XGBoost." },
  { "en": "Apa itu 'stacking'?", "id": "Ensemble yang gunakan model lain." },
  { "en": "Tugas 'outlier detection'?", "id": "Mendeteksi data yang aneh/berbeda." },
  { "en": "Penyebab 'outlier'?", "id": "Kesalahan input, kejadian langka." },
  { "en": "Apa itu 'principal components'?", "id": "Arah dengan varians data terbesar." },
  { "en": "Apa itu 'eigenvector' dan 'eigenvalue'?", "id": "Konsep matematika dasar untuk PCA." },
  { "en": "Beda 'PCA' dan 'LDA'?", "id": "LDA supervised, PCA unsupervised." },
  { "en": "Kepanjangan LDA (Linear Discriminant Analysis)?", "id": "Linear Discriminant Analysis." },
  { "en": "Tujuan LDA (Linear Discriminant Analysis)?", "id": "Maksimalkan pemisahan antar kelas." },
  { "en": "Metode 'elbow' untuk apa?", "id": "Menentukan jumlah cluster (K) optimal." },
  { "en": "Metode 'silhouette' untuk apa?", "id": "Mengukur kualitas hasil clustering." },
  { "en": "Apa itu 'DBSCAN'?", "id": "Algoritma clustering berbasis kepadatan." },
  { "en": "Keuntungan 'DBSCAN'?", "id": "Bisa temukan cluster bentuk aneh." },
  { "en": "Apa itu 'market basket analysis'?", "id": "Analisa pola pembelian pelanggan." },
  { "en": "Metrik pada 'association rule'?", "id": "Support, confidence, lift." },
  { "en": "Apa itu 'support'?", "id": "Frekuensi kemunculan item dalam data." },
  { "en": "Apa itu 'confidence'?", "id": "Probabilitas kondisional terjadinya aturan." },
  { "en": "Apa itu 'lift'?", "id": "Peningkatan probabilitas akibat aturan." },
  { "en": "Nilai 'lift' > 1 artinya?", "id": "Item-item cenderung dibeli bersama." },
  { "en": "Apa itu 'vanishing gradient'?", "id": "Masalah gradien sangat kecil." },
  { "en": "Apa itu 'exploding gradient'?", "id": "Masalah gradien sangat besar." },
  { "en": "Apa itu 'gated recurrent unit' (GRU)?", "id": "Jenis RNN lebih sederhana dari LSTM." },
  { "en": "Kepanjangan GRU?", "id": "Gated Recurrent Unit." },
  { "en": "Beda 'LSTM' dan 'GRU'?", "id": "GRU punya lebih sedikit 'gate'." },
  { "en": "Apa itu 'bidirectional RNN'?", "id": "RNN yang proses data maju-mundur." },
  { "en": "Apa itu 'sequence-to-sequence' model?", "id": "Model untuk sekuens input-output." },
  { "en": "Aplikasi 'seq2seq' model?", "id": "Machine translation, text summarization." },
  { "en": "Apa itu 'text summarization'?", "id": "Membuat ringkasan teks otomatis." },
  { "en": "Tipe 'text summarization'?", "id": "Extractive dan abstractive." },
  { "en": "Beda 'extractive' & 'abstractive'?", "id": "Extractive pilih, abstractive buat kalimat." },
  { "en": "Apa itu 'activation function' ReLU?", "id": "Mengubah nilai negatif menjadi nol." },
  { "en": "Kelebihan ReLU?", "id": "Sederhana, atasi 'vanishing gradient'." },
  { "en": "Kelemahan ReLU?", "id": "Dying ReLU problem." },
  { "en": "Apa itu 'dying ReLU problem'?", "id": "Neuron berhenti belajar selamanya." },
  { "en": "Variasi dari ReLU?", "id": "Leaky ReLU, Parametric ReLU." },
  { "en": "Apa itu 'knowledge graph'?", "id": "Graf yang merepresentasikan pengetahuan." },
  { "en": "Entitas dalam 'knowledge graph'?", "id": "Node (objek) dan edge (relasi)." },
  { "en": "Aplikasi 'knowledge graph'?", "id": "Pencarian semantik, sistem rekomendasi." },
  { "en": "Apa itu 'recommender system'?", "id": "Sistem yang merekomendasikan item." },
  { "en": "Tipe 'recommender system'?", "id": "Collaborative filtering, content-based." },
  { "en": "Prinsip 'collaborative filtering'?", "id": "Rekomendasi dari kemiripan pengguna/item." },
  { "en": "Prinsip 'content-based filtering'?", "id": "Rekomendasi dari kemiripan atribut item." },
  { "en": "Apa itu 'cold start problem'?", "id": "Masalah rekomendasi untuk user/item baru." },
  { "en": "Apa itu 'serendipity' rekomendasi?", "id": "Memberi rekomendasi yang mengejutkan." },
  { "en": "Apa itu 'dropout'?", "id": "Teknik regularisasi mematikan neuron acak." },
  { "en": "Apa itu 'fully connected layer'?", "id": "Setiap neuron terhubung ke semua neuron." },
  { "en": "Nama lain 'fully connected layer'?", "id": "Dense layer." },
  { "en": "Apa itu 'convolutional layer'?", "id": "Lapisan yang melakukan operasi konvolusi." },
  { "en": "Apa itu 'filter' atau 'kernel'?", "id": "Matriks 'weights' pada convolutional layer." },
  { "en": "Apa itu 'pooling layer'?", "id": "Lapisan untuk 'downsampling' fitur map." },
  { "en": "Apa itu 'object permanence'?", "id": "Masalah analitik saat objek terhalang." },
  { "en": "Apa itu 'occlusion'?", "id": "Objek terhalang oleh objek lain." },
  { "en": "Apa itu 'zero-padding'?", "id": "Menambahkan piksel nol di sekitar gambar." },
  { "en": "Apa itu 'batch' dalam training?", "id": "Sub-set dari data training." },
  { "en": "Apa itu 'stochastic gradient descent' (SGD)?", "id": "Gradient descent dengan batch size 1." },
  { "en": "Apa itu 'mini-batch gradient descent'?", "id": "Gradient descent dengan batch kecil." },
  { "en": "Efek 'batch size' besar?", "id": "Training lebih stabil, butuh memori." },
  { "en": "Efek 'batch size' kecil?", "id": "Training lebih berisik, konvergen cepat." },
  { "en": "Apa itu 'learning rate decay'?", "id": "Menurunkan learning rate seiring waktu." },
  { "en": "Apa itu 'data augmentation'?", "id": "Memperbanyak data latih secara sintetis." },
  { "en": "Contoh 'data augmentation' gambar?", "id": "Rotasi, flip, zoom, ubah warna." },
  { "en": "Apa itu 'data normalization'?", "id": "Mengubah data ke distribusi standar." },
  { "en": "Tujuan 'data normalization'?", "id": "Membantu model konvergen lebih cepat." },
  { "en": "Apa itu 'one-shot learning'?", "id": "Belajar dari satu contoh saja." },
  { "en": "Apa itu 'few-shot learning'?", "id": "Belajar efektif dari sedikit contoh." },
  { "en": "Apa itu 'zero-shot learning'?", "id": "Mengenali objek tanpa melihat contoh." },
  { "en": "Bagaimana 'zero-shot learning' bekerja?", "id": "Menggunakan atribut atau deskripsi." },
  { "en": "Apa itu 'meta-learning'?", "id": "Belajar cara untuk belajar." },
  { "en": "Apa itu 'imitation learning'?", "id": "Belajar dengan meniru ahli." },
  { "en": "Apa itu 'inverse reinforcement learning'?", "id": "Menemukan 'reward function' dari observasi." },
  { "en": "Apa itu 'multi-agent reinforcement learning'?", "id": "Banyak agent belajar bersama." },
  { "en": "Apa itu 'adversarial example'?", "id": "Input yang dirancang menipu model." },
  { "en": "Metode serangan 'adversarial'?", "id": "Fast Gradient Sign Method (FGSM)." },
  { "en": "Apa itu 'defensive distillation'?", "id": "Teknik pertahanan terhadap adversarial." },
  { "en": "Apa itu 'federated learning'?", "id": "Melatih model tanpa kumpulkan data." },
  { "en": "Manfaat 'federated learning'?", "id": "Menjaga privasi data pengguna." },
  { "en": "Apa itu 'swarm intelligence'?", "id": "Kecerdasan kolektif dari sistem terdesentralisasi." },
  { "en": "Contoh algoritma 'swarm intelligence'?", "id": "Ant Colony Optimization, Particle Swarm." },
  { "en": "Apa itu 'genetic algorithm'?", "id": "Algoritma optimisasi terinspirasi evolusi." },
  { "en": "Proses dalam 'genetic algorithm'?", "id": "Seleksi, crossover, dan mutasi." },
  { "en": "Apa itu 'fuzzy logic'?", "id": "Logika yang menangani ketidakpastian." },
  { "en": "Beda 'fuzzy logic' & 'boolean logic'?", "id": "Fuzzy punya nilai kebenaran parsial." },
  { "en": "Apa itu 'membership function'?", "id": "Fungsi dalam 'fuzzy logic'." },
  { "en": "Apa itu 'defuzzification'?", "id": "Mengubah hasil fuzzy jadi nilai tegas." },
  { "en": "Apa itu 'gradient tape'?", "id": "Fitur di TensorFlow untuk lacak gradien." },
  { "en": "Apa itu 'PyTorch Autograd'?", "id": "Fitur di PyTorch untuk lacak gradien." },
  { "en": "Apa itu 'tensor'?", "id": "Struktur data fundamental dalam deep learning." },
  { "en": "Skalar adalah tensor rank?", "id": "Rank 0." },
  { "en": "Vektor adalah tensor rank?", "id": "Rank 1." },
  { "en": "Matriks adalah tensor rank?", "id": "Rank 2." },
  { "en": "Framework deep learning populer?", "id": "TensorFlow, PyTorch, Keras." },
  { "en": "Apa itu 'Keras'?", "id": "API high-level untuk TensorFlow." },
  { "en": "Apa itu 'computational graph'?", "id": "Representasi graf dari operasi matematika." },
  { "en": "Jenis 'computational graph' PyTorch?", "id": "Graf dinamis." },
  { "en": "Jenis 'computational graph' TensorFlow 1?", "id": "Graf statis." },
  { "en": "Keuntungan graf dinamis?", "id": "Lebih fleksibel untuk debugging." },
  { "en": "Apa itu 'eager execution'?", "id": "Operasi dieksekusi langsung." },
  { "en": "Framework mana yang 'eager by default'?", "id": "PyTorch dan TensorFlow 2." },
  { "en": "Apa itu 'CUDA (Compute Unified Device Architecture)'?", "id": "Platform komputasi paralel dari Nvidia." },
  { "en": "Kepanjangan CUDA?", "id": "Compute Unified Device Architecture." },
  { "en": "Apa itu 'cuDNN'?", "id": "Library deep learning dari Nvidia." },
  { "en": "Kepanjangan cuDNN?", "id": "CUDA Deep Neural Network library." },
  { "en": "Apa itu 'OpenCV'?", "id": "Library open-source untuk computer vision." },
  { "en": "Fungsi 'OpenCV'?", "id": "Pemrosesan gambar dan video." },
  { "en": "Apa itu 'Pillow'?", "id": "Library manipulasi gambar di Python." },
  { "en": "Apa itu 'Scikit-learn'?", "id": "Library machine learning di Python." },
  { "en": "Fokus 'Scikit-learn'?", "id": "Algoritma machine learning tradisional." },
  { "en": "Apa itu 'Pandas'?", "id": "Library manipulasi data di Python." },
  { "en": "Apa itu 'NumPy'?", "id": "Library komputasi numerik di Python." },
  { "en": "Struktur data utama 'NumPy'?", "id": "Array multi-dimensi." },
  { "en": "Apa itu 'matplotlib'?", "id": "Library visualisasi data di Python." },
  { "en": "Apa itu 'seaborn'?", "id": "Library visualisasi di atas matplotlib." },
  { "en": "Apa itu 'Jupyter Notebook'?", "id": "Lingkungan interaktif untuk coding." },
  { "en": "Apa itu 'Google Colab'?", "id": "Jupyter Notebook gratis dengan GPU." },
  { "en": "Apa itu 'Kaggle'?", "id": "Platform kompetisi data science." },
  { "en": "Apa itu 'model checkpointing'?", "id": "Menyimpan model secara periodik." },
  { "en": "Tujuan 'model checkpointing'?", "id": "Melanjutkan training jika terputus." },
  { "en": "Apa itu 'transfer learning'?", "id": "Menggunakan model terlatih untuk tugas baru." },
  { "en": "Model terlatih disebut?", "id": "Pre-trained model." },
  { "en": "Apa itu 'fine-tuning'?", "id": "Melatih ulang beberapa layer terakhir." },
  { "en": "Apa itu 'feature extraction' (transfer learning)?", "id": "Hanya melatih layer output." },
  { "en": "Apa itu 'domain adaptation'?", "id": "Adaptasi model ke domain data baru." },
  { "en": "Apa itu 'data drift'?", "id": "Perubahan distribusi data seiring waktu." },
  { "en": "Apa itu 'concept drift'?", "id": "Perubahan hubungan antara fitur-label." },
  { "en": "Apa itu 'bias' dalam statistik?", "id": "Error dari asumsi yang salah." },
  { "en": "Apa itu 'variance' dalam statistik?", "id": "Error dari sensitivitas pada data." },
  { "en": "Tujuan regularisasi?", "id": "Mengurangi kompleksitas model, cegah overfitting." },
  { "en": "Apa itu 'greedy search'?", "id": "Algoritma yang ambil pilihan terbaik lokal." },
  { "en": "Apa itu 'beam search'?", "id": "Algoritma pencarian yang lebih baik." },
  { "en": "Penggunaan 'beam search'?", "id": "Machine translation, speech recognition." },
  { "en": "Apa itu 'sparse data'?", "id": "Data yang sebagian besar nilainya nol." },
  { "en": "Tantangan 'sparse data'?", "id": "Membutuhkan metode penyimpanan efisien." },
  { "en": "Apa itu 'time series data'?", "id": "Data yang diurutkan berdasarkan waktu." },
  { "en": "Contoh 'time series data'?", "id": "Harga saham, data cuaca." },
  { "en": "Tugas pada 'time series data'?", "id": "Forecasting (peramalan)." },
  { "en": "Model untuk 'time series forecasting'?", "id": "ARIMA, LSTM, Prophet." },
  { "en": "Apa itu 'causal inference'?", "id": "Menyimpulkan hubungan sebab-akibat dari data." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
