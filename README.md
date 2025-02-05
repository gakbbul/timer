<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>시간 카운트다운</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: white;
            color: black;
        }
        #main-timer {
            font-size: 3em;
            margin-bottom: 40px; /* 간격 증가 */
        }
        #next-timer {
            font-size: 1.5em;
            color: gray;
        }
        @media (prefers-color-scheme: dark) {
            body {
                background-color: black;
                color: white;
            }
            #next-timer {
                color: lightgray;
            }
        }
    </style>
</head>
<body>
    <div id="main-timer">계산 중...</div>
    <div id="next-timer">계산 중...</div>
    
    <script>
        function getNextTargetTimes() {
            const now = new Date();
            const targetDays = [1, 3, 5]; // 월(1), 수(3), 금(5)
            const targetHour = 21;
            const targetMinute = 25;

            let firstTarget = null;
            let secondTarget = null;

            for (let i = 0; i < 7; i++) {
                let tempDate = new Date(now);
                tempDate.setDate(now.getDate() + i);
                tempDate.setHours(targetHour, targetMinute, 0, 0);

                if (targetDays.includes(tempDate.getDay())) {
                    if (!firstTarget) {
                        if (tempDate > now) {
                            firstTarget = tempDate;
                        }
                    } else if (!secondTarget) {
                        if (tempDate > firstTarget) {
                            secondTarget = tempDate;
                            break;
                        }
                    }
                }
            }
            return { firstTarget, secondTarget };
        }

        function updateCountdown() {
            const { firstTarget, secondTarget } = getNextTargetTimes();
            const now = new Date();

            function formatTimeDiff(target) {
                const diff = target - now;
                const days = Math.floor(diff / (1000 * 60 * 60 * 24));
                const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((diff % (1000 * 60)) / 1000);
                
                let result = [];
                if (days > 0) result.push(`${days}일`);
                if (hours > 0) result.push(`${hours}시간`);
                if (minutes > 0) result.push(`${minutes}분`);
                result.push(`${seconds}초`); // 0초도 항상 표시
                
                return result.join(" ");
            }

            document.getElementById("main-timer").textContent = formatTimeDiff(firstTarget);
            document.getElementById("next-timer").textContent = `다음: ${formatTimeDiff(secondTarget)}`;
        }

        setInterval(updateCountdown, 1000);
        updateCountdown();
    </script>
</body>
</html>
