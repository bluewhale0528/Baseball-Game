## 1단계 : 간단 야구 게임 구현하기
1. S,B,O,H 중 랜덤으로 선택하는 코드

        function selectRandomCase() {
            var arr = ["S","B","O","H"];
            var random = arr[Math.floor(Math.random()*4)];
            return random;
        }
        
2. 각각 누적합을 저장하는 코드

        var count_S = 0; var count_B = 0; var count_H = 0; var count_O = 0;
        
        function checkStrike() {
            count_S += 1; if(count_S === 3) {
                count_O += 1;
                count_S = 0; 
            } return [count_S,count_B,count_H,count_O];
        }
        function checkBall() {
            count_B += 1; if(count_B === 4) {
                count_H += 1;
                count_B = 0;        
            } return [count_S,count_B,count_H,count_O];
        }
        function checkHit() {
            count_H += 1;
            count_S = 0;
            count_B = 0;
            return [count_S,count_B,count_H,count_O];       
        }
        function checkOut() {
            count_O += 1;
            count_S = 0;
            count_B = 0;        
            return [count_S,count_B,count_H,count_O];         
        }
        
3. S,B,H,O가 각각 출력되었을 때 어떠한 메시지를 출력할 것인지에 대한 코드

        function record(score) {
            if(score === "S") {
                checkStrike();
                document.write("스트라이크!<br>" + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            } else if(score === "B") {
                checkBall();
                document.write("볼!<br>" + + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            } else if(score === "H") {
                checkHit();
                document.write("안타! 다음 타자가 타석에 입장했습니다.<br>" + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            } else {
                checkOut();
                document.write("아웃! 다음 타자가 타석에 입장했습니다.<br>" + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            }
        }
        
4. 최종 안타수 출력 및 결과물 화면창에 출력하는 코드

        function print(value) {
            document.write("최종 안타수 : " + count_H  + "<br>" + "GAME OVER");
        }
        
        function runGame() {
            document.write("첫 번째 타자가 타석에 입장했습니다.<br><br>")
            while(count_O < 3) {
                var score = selectRandomCase();
                record(score);
            } print(count_H);
        }
        
## 2단계 : 팀데이터 입력 및 시합 기능 구현
1. 팀 데이터 입력하는 코드

        var teamA = {};
        var teamB = {};
        function inputName(team) {
            while (true) {
                if (team === teamA) {
                    var i = 1;
                } else {
                    i = 2;
                }
                var str1 = prompt(i + "팀의 이름을 입력하세요");
                if (str1.length != 0) {
                    team.name = str1;
                    break;
                } else {
                    alert("에러! 다시입력하세요");
                }
            } return team;
        }
        
        function inputplayer(team) {
            for (var i = 1; i <= 9; i++) {
                while (true) {
                    var str = prompt(i + "번 타자 정보 입력 \n 형식 : 이름, 타율 (단, 타율은 0.1과 0.5 사이이고, 소수 셋째자리까지 입력)");
                    if (str.indexOf(",") != -1) {
                        var check = str.split(", ");
                        if (Number(check[1]) > 0.1 & Number(check[1]) < 0.5 & check[1].length === 5) {
                            team[i] = check;
                            team[i].unshift(i + "번");
                            break;
                        } else {
                            alert("에러! 조건에 맞게 다시 입력하세요");
                        }
                    } else {
                        alert("에러! 조건에 맞게 다시 입력하세요");
                    }
                }
            } return team;
        }
        
2. 입력받은 팀 정보를 콘솔창에 출력하는 코드

        function outputplayer(team) {
            for (var i = 1; i <= 9; i++) {
                console.log(team[i][0] + " " + team[i][1] + " " + team[i][2]);
            }
        }
        function output() {
            console.log(teamA.name + " 팀 정보");
            outputplayer(teamA);
            console.log("");
            console.log(teamB.name + " 팀 정보");
            outputplayer(teamB);
            console.log("");
            document.getElementById("output").innerHTML = "* 출력 결과는 콘솔창에 나타납니다."
            
3. 입력받은 타율을 기준으로 S,B,H,O 랜덤으로 선택하는 코드

        function getRandomscore(team) {
            for (var i = 1; i <= 9; i++) {
                var random = Math.random().toFixed(3);
                var h = Number(team[i][2]);
                if (random <= h) {
                    team[i][3] = "H";
                } else if (random > h & random <= (h + (1 - h) / 2 - 0.05)) {
                    team[i][3] = "S";
                } else if (random >= 0.900) {
                    team[i][3] = "O";
                } else {
                    team[i][3] = "B";
                }
            } return team;
        }
        
4. 각 플레이어의 타율에 따라 결정된 S,H,B,O에 따른 문구 출력하는 코드

        teamA.score = [];
        teamB.score = [];
        var count_S = 0; var count_B = 0; var count_H = 0; var count_O = 0; var count_score = 0;
        var pre_O; var pre_H;
        function getStrike() {
            count_S += 1;
            pre_H = count_H;
            pre_O = count_O;
            if (count_S === 3) {
                pre_H = count_H;
                pre_O = count_O;
                count_O += 1;
                count_S = 0;
            } return [count_S, count_B, count_H, count_O];
        }
        function getBall() {
            count_B += 1;
            pre_H = count_H;
            pre_O = count_O;
            if (count_B === 4) {
                pre_H = count_H;
                pre_O = count_O;
                count_H += 1;
                count_B = 0;
            } return [count_S, count_B, count_H, count_O];
        }
        function getHit() {
            pre_H = count_H;
            pre_O = count_O;
            count_H += 1;
            count_S = 0;
            count_B = 0;
            if (count_H >= 4) {
                count_score += 1;
            }
            return [count_S, count_B, count_H, count_O];
        }
        function getOut() {
            pre_H = count_H;
            pre_O = count_O;
            count_O += 1;
            count_S = 0;
            count_B = 0;
            return [count_S, count_B, count_H, count_O];
        }
        function record(score, count_score) {
            if (score === "S") {
                getStrike();
                document.write("스트라이크!<br>"+ count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            } else if (score === "B") {
                getBall();
                document.write("볼!<br>" + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            } else if (score === "H") {
                getHit();
                document.write("안타!<br>" + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            } else {
                getOut();
                document.write("아웃!<br>" + count_S + "S " + count_B + "B " + count_O + "O<br><br>");
            }
        }
        
5. 시합 과정을 출력해주는 코드


        function game(n, team) {
            if (team === "teamA") {
                var time = "회초 ";
            } else {
                time = "회말 ";
            }
            document.write(n + time + team.name + " 공격<br><br>");
            var i = 1;
            while (count_O < 3) {
                var team_input = getRandomscore(team)
                var score = team_input[i][3];
                document.write(team[i][0] + " " + team[i][1] + "<br>");
                record(score, count_score);
                if (count_O != pre_O || count_H != pre_H) {
                    i += 1;
                    if (i === 10) {
                        i = 1;
                    }
                }
            }
            count_S = 0; count_B = 0; count_H = 0; count_O = 0; team.score.push(count_score);
            count_score = 0;
        }
        
6. 각 회마다 각 팀이 얻게 되는 스코어를 저장해주는 코드

        function finalScore(team) {
            var sum = 0
            for (i = 0; i <= 5; i++) {
                sum += team.score[i]
            } return sum;
        }
        
7. 최종 각 팀이 얻게 된 스코어를 기반으로 시합 결과를 출력해주는 코드

        function result() {
            document.write("경기종료<br><br>");
            document.write(teamA.name + " vs " + teamB.name + "<br>");
            document.write(finalScore(teamA) + " : " + finalScore(teamB));
            if (finalScore(teamA) > finalScore(teamB)) {
                txt += teamA.name + "의 승리입니다.";
            } else if (finalScore(teamA) === finalScore(teamB)) {
                txt += "양팀 무승부입니다.";
            } else {
                txt += teamB.name + "의 승리입니다.";
            }
            document.write("<br>Thank you!")
        }
        
8. 실제 시합 실행하는 코드

                function runGame() {
            document.write(teamA.name + " vs " + teamB.name + "의 시합을 시작합니다.<br><br>")
            for (var i = 1; i <= 6; i++) {
                game(i, teamA);
                game(i, teamB);
            }
            result();
            console.log(txt);
        }    
