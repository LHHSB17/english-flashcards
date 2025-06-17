# english-flashcards
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>英文單字卡</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Light blue-gray background */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .flashcard-container {
            perspective: 1000px; /* For 3D flip effect */
            width: 100%;
            max-width: 500px; /* Max width for larger screens */
            min-height: 300px; /* Minimum height for better appearance */
            margin: 0 auto;
        }
        .flashcard {
            width: 100%;
            height: 100%;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            cursor: pointer;
            border-radius: 1.5rem; /* Rounded corners */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); /* Soft shadow */
        }
        .flashcard.flipped {
            transform: rotateY(180deg);
        }
        .flashcard-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden; /* Hide back face during flip */
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 2rem;
            text-align: center;
            border-radius: 1.5rem; /* Rounded corners */
            background-color: #ffffff; /* White card background */
        }
        .flashcard-front {
            z-index: 2;
            transform: rotateY(0deg);
            color: #1a202c; /* Dark text for readability */
        }
        .flashcard-back {
            transform: rotateY(180deg);
            color: #1a202c; /* Dark text for readability */
        }
        .flashcard-word {
            font-size: 2.5rem; /* Large font for the word */
            font-weight: bold;
            color: #3182ce; /* Blue for the word */
            margin-bottom: 1rem;
        }
        .flashcard-translation {
            font-size: 1.5rem;
            color: #4a5568; /* Gray for translation */
            margin-bottom: 0.5rem;
        }
        .flashcard-example {
            font-size: 1rem;
            font-style: italic;
            color: #718096; /* Lighter gray for example */
            line-height: 1.5;
        }
        .navigation-buttons {
            display: flex;
            justify-content: center;
            gap: 1.5rem; /* Space between buttons */
            margin-top: 2rem;
            width: 100%;
            max-width: 500px;
        }
        .nav-button {
            background-color: #4299e1; /* Blue button */
            color: white;
            padding: 1rem 2rem;
            border-radius: 9999px; /* Fully rounded buttons */
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.1s ease;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            border: none;
            outline: none;
            flex: 1; /* Make buttons fill available space */
        }
        .nav-button:hover {
            background-color: #3182ce; /* Darker blue on hover */
            transform: translateY(-2px); /* Slight lift on hover */
        }
        .nav-button:active {
            transform: translateY(0); /* Press effect */
        }

        /* Responsive adjustments */
        @media (max-width: 640px) {
            .flashcard-word {
                font-size: 2rem;
            }
            .flashcard-translation {
                font-size: 1.25rem;
            }
            .flashcard-example {
                font-size: 0.9rem;
            }
            .flashcard-face {
                padding: 1.5rem;
            }
            .nav-button {
                padding: 0.8rem 1.5rem;
                font-size: 0.9rem;
            }
            .navigation-buttons {
                flex-direction: column; /* Stack buttons on small screens */
                gap: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="flex flex-col items-center w-full">
        <h1 class="text-3xl md:text-4xl font-extrabold text-gray-800 mb-8 text-center">英文單字卡</h1>

        <div class="flashcard-container" style="height: 300px;">
            <div id="flashcard" class="flashcard">
                <div class="flashcard-face flashcard-front">
                    <div id="word" class="flashcard-word"></div>
                </div>
                <div class="flashcard-face flashcard-back">
                    <div id="translation" class="flashcard-translation"></div>
                    <p id="example" class="flashcard-example mt-4"></p>
                </div>
            </div>
        </div>

        <div class="navigation-buttons">
            <button id="prevBtn" class="nav-button">上一個</button>
            <button id="flipBtn" class="nav-button">翻轉</button>
            <button id="nextBtn" class="nav-button">下一個</button>
        </div>
    </div>

    <script>
        // Vocabulary data from the provided document
        const vocabulary = [
            {
                word: "Cooperation",
                translation: "合作",
                example: "Each of these recent administrations deepened diplomatic, technological, and military cooperation with India..."
            },
            {
                word: "Rationale",
                translation: "理由、基本原理",
                example: "The rationale for this pledge was simple."
            },
            {
                word: "Challenges",
                translation: "挑戰",
                example: "The Chinese economy is buffeted by multiple challenges that could keep growth rates down..."
            },
            {
                word: "Aligned",
                translation: "一致的、結盟的",
                example: "But India and the United States are not aligned on all issues."
            },
            {
                word: "Multipolar",
                translation: "多極化的",
                example: "Instead, it seeks a multipolar international system, in which India would rank as a genuine great power."
            },
            {
                word: "Autonomy",
                translation: "自主權、自治",
                example: "It obsessively guards its strategic autonomy, eschewing formal alliances and maintaining ties with Western adversaries such as Iran and Russia..."
            },
            {
                word: "Insufficient",
                translation: "不足的",
                example: "...its pace is insufficient to balance China, let alone the United States."
            },
            {
                word: "Rival",
                translation: "競爭對手",
                example: "...its advantages over its local rival are not enormous..."
            },
            {
                word: "Undermines",
                translation: "削弱、破壞",
                example: "This evolution could undermine India's rise by intensifying communal tensions and exacerbating problems with its neighbors..."
            },
            {
                word: "Trajectory",
                translation: "軌跡、發展路徑",
                example: "India's relative weakness, its yearning for multipolarity, and its illiberal trajectory mean that it will have less global influence than it desires..."
            },
            {
                word: "Influence",
                translation: "影響力",
                example: "...its global influence will fall short of its increasing material strength."
            },
            {
                word: "Protectionism",
                translation: "保護主義",
                example: "...clings to excessive protectionism that impedes exports..."
            },
            {
                word: "Constrain",
                translation: "限制、約束",
                example: "If New Delhi wants to constrain Beijing, it will therefore need Washington."
            },
            {
                word: "Partnership",
                translation: "夥伴關係",
                example: "...India's diffidence about a tight partnership with the United States frustrates this outcome."
            },
            {
                word: "Polarization",
                translation: "兩極分化",
                example: "The BJP's policies have polarized India along ideological and religious lines..."
            },
            {
                word: "Delusions",
                translation: "幻想、妄想",
                example: "India's Great-Power Delusions"
            },
            {
                word: "Strategic",
                translation: "戰略的",
                example: "ASHLEY J. TELLIS is the Tata Chair for Strategic Affairs and a Senior Fellow at the Carnegie Endowment for International Peace."
            },
            {
                word: "Capabilities",
                translation: "能力、潛力",
                example: "Under the Obama administration, the United States and India began defense industrial cooperation that aimed to boost the latter's military capabilities and help it project power."
            },
            {
                word: "Objective",
                translation: "目標、宗旨",
                example: "For the foreseeable future, India will find it hard to achieve either objective."
            },
            {
                word: "Compensate",
                translation: "彌補、補償",
                example: "To compensate, India would have to bear larger financial and security burdens than it has been willing to thus far."
            }
        ];

        let currentIndex = 0; // Current index of the word being displayed
        let isFlipped = false; // Tracks if the card is flipped to the back

        const flashcardElement = document.getElementById('flashcard');
        const wordElement = document.getElementById('word');
        const translationElement = document.getElementById('translation');
        const exampleElement = document.getElementById('example');
        const flipBtn = document.getElementById('flipBtn');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');

        // Function to display the current word and its information
        function displayCard(index) {
            const cardData = vocabulary[index];
            wordElement.textContent = cardData.word;
            translationElement.textContent = cardData.translation;
            exampleElement.textContent = cardData.example;

            // Reset flip state when changing cards
            if (isFlipped) {
                flashcardElement.classList.remove('flipped');
                isFlipped = false;
            }
        }

        // Event listener for flipping the card
        flashcardElement.addEventListener('click', () => {
            flashcardElement.classList.toggle('flipped');
            isFlipped = !isFlipped;
        });

        // Event listener for the flip button
        flipBtn.addEventListener('click', (e) => {
            e.stopPropagation(); // Prevent card click event from firing simultaneously
            flashcardElement.classList.toggle('flipped');
            isFlipped = !isFlipped;
        });

        // Event listener for the previous button
        prevBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            currentIndex = (currentIndex === 0) ? vocabulary.length - 1 : currentIndex - 1;
            displayCard(currentIndex);
        });

        // Event listener for the next button
        nextBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            currentIndex = (currentIndex === vocabulary.length - 1) ? 0 : currentIndex + 1;
            displayCard(currentIndex);
        });

        // Initial display of the first card
        window.onload = function() {
            displayCard(currentIndex);
        };
    </script>
</body>
</html>
