<!DOCTYPE html>
<html>
<head>
    <style>
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="myCanvas" width="400" height="400"></canvas>

    <script>
        var canvas = document.getElementById('myCanvas');
        var ctx = canvas.getContext('2d');

        //棋盘大小
        var size = 8;

        //棋子颜色
        var colors = ['black', 'red'];

        //当前玩家的颜色
        var currentPlayerColor = colors[0];

        //绘制棋盘
        function drawBoard() {
            for (var i = 0; i <= size; i++) {
                ctx.moveTo(0, i * (canvas.height / size));
                ctx.lineTo(canvas.width, i * (canvas.height / size));
                ctx.stroke();

                ctx.moveTo(i * (canvas.width / size), 0);
                ctx.lineTo(i * (canvas.width / size), canvas.height);
                ctx.stroke();
            }
        }

        //在指定位置画一个棋子
        function drawPiece(x, y) {
            ctx.beginPath();
            ctx.arc(x * (canvas.width / size) + (canvas.width / size) / 2, y * (canvas.height / size) + (canvas.height / size) / 2, (canvas.width / size) / 3, 0, Math.PI*2);
            ctx.fillStyle = currentPlayerColor;
            ctx.fill();
            ctx.closePath();
        }

        //检查是否有5个连续的同色棋子
        function checkWin(color) {
            //检查行
            for (var i = 0; i < size; i++) {
                for (var j = 0; j < size - 4; j++) {
                    if (color === board[i][j] && color === board[i][j+1] && color === board[i][j+2] && color === board[i][j+3] && color === board[i][j+4]) {
                        return true;
                    }
                }
            }

            //检查列
            for (var i = 0; i < size; i++) {
                for (var j = 0; j < size - 4; j++) {
                    if (color === board[j][i] && color === board[j+1][i] && color === board[j+2][i] && color === board[j+3][i] && color === board[j+4][i]) {
                        return true;
                    }
                }
            }

            //检查对角线
            for (var i = 0; i < size - 4; i++) {
                for (var j = 0; j < size - 4; j++) {
                    if (color === board[i][j] && color === board[i+1][j+1] && color === board[i+2][j+2] && color === board[i+3][j+3] && color === board[i+4][j+4]) {
                        return true;
                    }
                    if (color === board[i][j+4] && color === board[i+1][j+3] && color === board[i+2][j+2] && color === board[i+3][j+1] && color === board[i+4][j]) {
                        return true;
                    }
                }
            }

            return false;
        }

        //初始化棋盘
        var board = [];
        for (var i = 0; i < size; i++) {
            board[i] = new Array(size).fill(null);
        }

        drawBoard();

        canvas.addEventListener('click', function(event) {
            var rect = canvas.getBoundingClientRect();
            var x = event.clientX - rect.left;
            var y = event.clientY - rect.top;
            var column = Math.floor(x / (canvas.width / size));
            var row = Math.floor(y / (canvas.height / size));

            if (board[row][column] === null) {
                board[row][column] = currentPlayerColor;
                drawPiece(column, row);

                if (checkWin(currentPlayerColor)) {
                    alert(currentPlayerColor + '获胜！');
                } else {
                    currentPlayerColor = colors[(colors.indexOf(currentPlayerColor) + 1) % 2];
                }
            }
        });
    </script>
</body>
</html>
