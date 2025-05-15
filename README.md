<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Kuis Analisis Kulit</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      background: linear-gradient(135deg, #ffe4ec, #fce4ec);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    .container {
      max-width: 600px;
      width: 100%;
      background: #fff0f5;
      border-radius: 20px;
      box-shadow: 0 8px 20px rgba(255, 105, 180, 0.2);
      overflow: hidden;
      padding: 30px;
      text-align: center;
      animation: fadeIn 0.6s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    h1 {
      font-size: 26px;
      color: #d63384;
      margin-bottom: 20px;
    }

    p {
      font-size: 16px;
      color: #6c757d;
      margin-bottom: 25px;
    }

    .btn {
      background: linear-gradient(135deg, #ff69b4, #ff85c1);
      color: white;
      padding: 12px 24px;
      border: none;
      border-radius: 30px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      margin: 10px;
      transition: transform 0.2s ease, box-shadow 0.3s ease;
    }

    .btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 16px rgba(255, 105, 180, 0.3);
    }
    
.btn.back {
  background: #f8d7da;
  color: #c2185b;
}

.btn.back:hover {
  background: #f5c6cb;
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(200, 35, 91, 0.2);
}
    

    .question {
      font-size: 18px;
      color: #c2185b;
      margin-bottom: 20px;
      font-weight: 600;
    }

    .options {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .option {
      background-color: #fff;
      padding: 15px;
      border-radius: 12px;
      cursor: pointer;
      border: 2px solid transparent;
      transition: all 0.3s ease;
      font-size: 15px;
      color: #555;
    }

    .option:hover {
      background-color: #ffe4ec;
    }

    .option.selected {
      background-color: #f8bbd0;
      border-color: #ec407a;
      color: #c2185b;
    }

    .intro, .quiz, .result {
      display: none;
    }

    .intro.active, .quiz.active, .result.active {
      display: block;
    }

    .result p {
      font-size: 18px;
      color: #444;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Halaman Pengantar -->
    <div class="intro active">
      <h1>Selamat Datang di Kuis Analisis Kulit</h1>
      <p>Ikuti kuis ini untuk mengetahui jenis kulitmu dan dapatkan rekomendasi perawatan yang sesuai!</p>
      <button class="btn" onclick="startQuiz()">Mulai Kuis</button>
    </div>

    <!-- Halaman Kuis -->
    <div class="quiz">
      <div class="question"></div>
      <div class="options"></div>
      <div>
        <button class="btn back" onclick="prevQuestion()">Kembali</button>
        <button class="btn" onclick="nextQuestion()">Lanjut</button>
      </div>
    </div>

    <!-- Halaman Hasil -->
    <div class="result">
      <h1>Hasil Analisis Kulit</h1>
      <p id="resultText"></p>
      <button class="btn" onclick="restartQuiz()">Ulangi Kuis</button>
    </div>
  <div>
<button class="btn back" onclick="location.href='hal6.html'">Kembali ke Halaman Sebelumnya</button></div>
  <script>
    const questions = [
      {
        question: "Bagaimana tekstur kulit wajahmu setelah mencuci muka?",
        options: ["Kencang dan kering", "Lembut dan seimbang", "Berminyak di beberapa area", "Sangat berminyak"],
        scores: [1, 2, 3, 4]
      },
      {
        question: "Seberapa sering kamu mengalami jerawat?",
        options: ["Jarang", "Kadang-kadang", "Sering", "Hampir selalu"],
        scores: [1, 2, 3, 4]
      },
      {
        question: "Bagaimana pori-pori di wajahmu?",
        options: ["Kecil dan tidak terlihat", "Sedang", "Besar di beberapa area", "Sangat besar"],
        scores: [1, 2, 3, 4]
      }
    ];

    let currentQuestion = 0;
    let answers = [];
    const intro = document.querySelector('.intro');
    const quiz = document.querySelector('.quiz');
    const result = document.querySelector('.result');
    const questionElement = document.querySelector('.question');
    const optionsElement = document.querySelector('.options');
    const resultText = document.getElementById('resultText');

    function startQuiz() {
      intro.classList.remove('active');
      quiz.classList.add('active');
      showQuestion();
    }

    function showQuestion() {
      const q = questions[currentQuestion];
      questionElement.textContent = q.question;
      optionsElement.innerHTML = '';
      q.options.forEach((option, index) => {
        const div = document.createElement('div');
        div.classList.add('option');
        div.textContent = option;
        div.onclick = () => selectOption(index, div);
        optionsElement.appendChild(div);
      });

      // Jika sebelumnya sudah memilih, tandai ulang
      if (answers[currentQuestion] !== undefined) {
        const selected = answers[currentQuestion];
        const options = optionsElement.querySelectorAll('.option');
        options[selected - 1].classList.add('selected');
      }
    }

    function selectOption(index, element) {
      answers[currentQuestion] = questions[currentQuestion].scores[index];
      const options = optionsElement.querySelectorAll('.option');
      options.forEach(opt => opt.classList.remove('selected'));
      element.classList.add('selected');
    }

    function nextQuestion() {
      if (!answers[currentQuestion]) {
        alert('Pilih salah satu opsi terlebih dahulu!');
        return;
      }
      currentQuestion++;
      if (currentQuestion < questions.length) {
        showQuestion();
      } else {
        showResult();
      }
    }

    function prevQuestion() {
      if (currentQuestion > 0) {
        currentQuestion--;
        showQuestion();
      }
    }

    function showResult() {
      quiz.classList.remove('active');
      result.classList.add('active');
      const totalScore = answers.reduce((sum, score) => sum + score, 0);
      let resultMessage = '';

      if (totalScore <= 4) {
        resultMessage = 'Kulitmu cenderung kering. Gunakan pelembap intensif dan hindari pembersih yang keras.';
      } else if (totalScore <= 7) {
        resultMessage = 'Kulitmu normal/seimbang. Pertahankan dengan rutinitas perawatan dasar.';
      } else if (totalScore <= 10) {
        resultMessage = 'Kulitmu kombinasi. Gunakan produk yang menyeimbangkan area berminyak dan kering.';
      } else {
        resultMessage = 'Kulitmu berminyak. Pilih pembersih lembut dan produk pengontrol minyak.';
      }

      resultText.textContent = resultMessage;
    }

    function restartQuiz() {
      currentQuestion = 0;
      answers = [];
      result.classList.remove('active');
      intro.classList.add('active');
    }
  </script>
 
</body>
</html>
