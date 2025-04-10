# word-block-game
!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>英単語ブロック崩し</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.22.6/babel.min.js"></script>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 5px; max-width: 300px; margin: auto; }
        .block { background: black; width: 100px; height: 100px; }
        .question { font-size: 20px; margin-top: 20px; }
        .options button { display: block; width: 200px; margin: 10px auto; padding: 10px; font-size: 18px; }
        .image-container { position: relative; display: inline-block; }
        .hidden-image { width: 300px; height: 300px; }
    </style>
</head>
<body>
    <div id="app"></div>

    <script type="text/babel">
        function App() {
            const [blocks, setBlocks] = React.useState(Array(9).fill(true));
            const questions = [
                { word: "apple", meaning: "りんご" },
                { word: "banana", meaning: "バナナ" },
                { word: "car", meaning: "車" },
                { word: "dog", meaning: "犬" },
                { word: "elephant", meaning: "象" },
                { word: "fish", meaning: "魚" },
                { word: "grape", meaning: "ぶどう" },
                { word: "house", meaning: "家" },
                { word: "ice", meaning: "氷" }
            ];
            const [question, setQuestion] = React.useState(questions[Math.floor(Math.random() * questions.length)]);
            
            function handleAnswer(answer) {
                if (answer === question.meaning) {
                    const newBlocks = [...blocks];
                    const remainingIndexes = newBlocks.map((v, i) => (v ? i : null)).filter(v => v !== null);
                    if (remainingIndexes.length > 0) {
                        const removeIndex = remainingIndexes[Math.floor(Math.random() * remainingIndexes.length)];
                        newBlocks[removeIndex] = false;
                        setBlocks(newBlocks);
                    }
                }
                setQuestion(questions[Math.floor(Math.random() * questions.length)]);
            }

            return (
                <div>
                    <h1>英単語ブロック崩し</h1>
                    <div className="image-container">
                        <img src="game_image.jpg" alt="Hidden" className="hidden-image" />
                        <div className="grid">
                            {blocks.map((visible, i) => visible ? <div key={i} className="block"></div> : null)}
                        </div>
                    </div>
                    <div className="question">{question.word} の意味は？</div>
                    <div className="options">
                        {[question.meaning, ...questions.filter(q => q !== question).map(q => q.meaning)]
                            .sort(() => Math.random() - 0.5)
                            .map((option, i) => (
                                <button key={i} onClick={() => handleAnswer(option)}>{option}</button>
                            ))}
                    </div>
                </div>
            );
        }

        ReactDOM.createRoot(document.getElementById("app")).render(<App />);
    </script>
</body>
</html>
