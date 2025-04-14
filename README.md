<!DOCTYPE html>
<html lang="kk">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Интерактивті Кроссворд</title>
  <style>
    body {
      font-family: sans-serif;
      background: #f5f0e6;
      color: #4b3b2a;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }
    h1 {
      color: #6b4c3b;
    }
    .crossword {
      display: grid;
      grid-template-columns: repeat(14, 40px);
      grid-auto-rows: 40px;
      gap: 2px;
      position: relative;
    }
    .cell {
      background: #fffaf3;
      border: 1px solid #d2b48c;
      text-align: center;
      vertical-align: middle;
      position: relative;
    }
    input {
      width: 100%;
      height: 100%;
      text-transform: uppercase;
      border: none;
      text-align: center;
      font-size: 18px;
      background: transparent;
    }
    .black {
      background: #8b6f47;
    }
    .number {
      position: absolute;
      top: 2px;
      left: 3px;
      font-size: 10px;
      color: #6b4c3b;
    }
    .hints {
      max-width: 600px;
      margin-top: 20px;
      background: #fffaf3;
      padding: 15px;
      border: 1px solid #d2b48c;
      border-radius: 8px;
    }
    .check-button {
      margin-top: 20px;
      padding: 10px 20px;
      background: #8b6f47;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Интерактивті Кроссворд</h1>
  <div class="crossword" id="crossword"></div>
  <button class="check-button" onclick="checkAnswers()">Тексеру</button>
  <div class="hints">
    <h3>По горизонтали:</h3>
    <p><strong>2.</strong> Белгілі бір тақырыпты тереңірек қарастырып, ғылыми түрде талдау үдерісі.</p>
    <p><strong>4.</strong> Мәтіндегі ойдың түсініктілігі, дәл және нақты түрде берілуі.</p>
    <p><strong>8.</strong> Жазбаның немесе пікірдің соңындағы түйін, жалпы тұжырым.</p>
    <p><strong>9.</strong> Ойдың жүйелілігі мен қисындылығы, тұжырымдардың бірізділігі.</p>
    <h3>По вертикали:</h3>
    <p><strong>1.</strong> Тезистің немесе мәтіннің белгілі бір тәртіппен орналасқан бөліктері.</p>
    <p><strong>3.</strong> Бір ұғымның немесе ойдың ішкі мағынасы, мазмұны.</p>
    <p><strong>5.</strong> Артық сөзсіз, ықшам түрде мазмұнды беру қасиеті.</p>
    <p><strong>6.</strong> Зерттеу немесе ой қорыту барысында алынған қорытынды мәлімет.</p>
    <p><strong>7.</strong> Автордың ойы, шығармашылық тұжырымдамасы немесе негізгі пікір.</p>
  </div>
  <script>
    const gridData = [
      [null,null,null,null,null,null,null,'Қ',null,null,null,null,null,null],
      [null,null,null,null,null,null,null,'Ұ',null,null,null,null,null,null],
      [null,null,null,null,null,'З','Е','Р','Т','Т','Е','У',null,null],
      [null,null,null,'М',null,null,null,'Ы',null,null,null,null,null,null],
      [null,null,null,'Ә',null,null,null,'Л',null,null,null,null,null,null],
      [null,null,'А','Н','Ы','Қ','Т','Ы','Қ',null,null,null,null,null],
      [null,null,null,'І',null,'Ы',null,'М',null,'Н',null,null,null,null],
      [null,null,null,null,null,'С',null,null,null,'Ә',null,'И',null,null],
      [null,null,null,null,null,'Қ','О','Р','Ы','Т','Ы','Н','Д','Ы',null],
      ['Л','О','Г','И','К','А',null,null,'И',null,'Е',null,null,null],
      [null,null,null,null,'Л',null,null,null,'Ж',null,'Я',null,null,null],
      [null,null,null,null,'Ы',null,null,null,'Е',null,null,null,null,null],
      [null,null,null,null,'Қ',null,null,null,null,null,null,null,null],
    ];

    const answers = [
  ['', '', '', '', '', '', '', 'Қ', '', '', '', '', '', ''],
  ['', '', '', '', '', '', '', 'Ұ', '', '', '', '', '', ''],
  ['', '', '', '', '', 'З', 'Е', 'Р', 'Т', 'Т', 'Е', 'У', '', ''],
  ['', '', '', 'М', '', '', '', 'Ы', '', '', '', '', '', ''],
  ['', '', '', 'Ә', '', '', '', 'Л', '', '', '', '', '', ''],
  ['', '', 'А', 'Н', 'Ы', 'Қ', 'Т', 'Ы', 'Қ', '', '', '', '', ''],
  ['', '', '', 'І', '', 'Ы', '', 'М', '', 'Н', '', '', '', ''],
  ['', '', '', '', '', 'С', '', '', '', 'Ә', '', 'И', '', ''],
  ['', '', '', '', '', 'Қ', 'О', 'Р', 'Ы', 'Т', 'Ы', 'Н', 'Д', 'Ы'],
  ['Л', 'О', 'Г', 'И', 'К', 'А', '', '', 'И', '', 'Е', '', '', ''],
  ['', '', '', '', '', 'Л', '', '', 'Ж', '', 'Я', '', '', ''],
  ['', '', '', '', '', 'Ы', '', '', 'Е', '', '', '', '', ''],
  ['', '', '', '', '', 'Қ', '', '', '', '', '', '', '', ''],
];


    const numbers = {
      '0,1': '1',
      '3,0': '2',
      '4,0': '4',
      '8,0': '8',
      '0,0': '9',
      '0,2': '3',
      '2,2': '5',
      '0,2': '6',
      '6,2': '7',
    };

    const crossword = document.getElementById('crossword');

    gridData.forEach((row, rowIndex) => {
      row.forEach((cell, colIndex) => {
        const cellDiv = document.createElement('div');
        cellDiv.classList.add('cell');

        if (cell === null) {
          cellDiv.classList.add('black');
        } else {
          if (numbers[`${rowIndex},${colIndex}`]) {
            const num = document.createElement('div');
            num.classList.add('number');
            num.textContent = numbers[`${rowIndex},${colIndex}`];
            cellDiv.appendChild(num);
          }

          const input = document.createElement('input');
          input.maxLength = 1;
          input.dataset.row = rowIndex;
          input.dataset.col = colIndex;
          cellDiv.appendChild(input);
        }

        crossword.appendChild(cellDiv);
      });
    });

    function checkAnswers() {
      const inputs = document.querySelectorAll('input');
      inputs.forEach(input => {
        const row = input.dataset.row;
        const col = input.dataset.col;
        const correctLetter = answers[row][col];
        if (input.value.toUpperCase() === correctLetter) {
          input.style.backgroundColor = '#c0e4c0';
        } else {
          input.style.backgroundColor = '#f4c2c2';
        }
      });
    }
  </script>
</body>
</html>
