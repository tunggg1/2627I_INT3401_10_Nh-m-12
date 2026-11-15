# Tìm kiếm và tìm kiếm đối kháng trong trò chơi Pacman

Bài tập lớn môn **Trí tuệ nhân tạo (INT3401_10)** – **Nhóm 12**

| Thành viên | MSSV |
|---|---|
| Nguyễn Sơn Tùng | 24023087 |
| Cao Thị Phương Anh | 24022935 |

Dự án cài đặt và đánh giá các thuật toán tìm kiếm và ra quyết định trên bộ khung Pacman của UC Berkeley CS188 (<https://ai.berkeley.edu>).

- **Project 1 – Tìm kiếm:** DFS, BFS, UCS, A\*, bài toán bốn góc, heuristic ăn hết thức ăn, tìm thức ăn gần nhất. Autograder: **26/25**.
- **Project 2 – Đối kháng:** ReflexAgent, Minimax, Alpha-Beta, Expectimax, hàm đánh giá tốt hơn. Autograder: **25/25**.

Báo cáo đầy đủ: `BaoCao.pdf`.

## Cấu trúc thư mục

```
.
├── pro1/search/search/          # Project 1
│   ├── search.py                # DFS, BFS, UCS, A*   (phần nhóm cài đặt)
│   ├── searchAgents.py          # CornersProblem, cornersHeuristic, foodHeuristic,
│   │                            # ClosestDotSearchAgent, AnyFoodSearchProblem
│   ├── autograder.py, layouts/, test_cases/ ...   # bộ khung của môn học
├── pro2/multiagent/multiagent/  # Project 2
│   ├── multiAgents.py           # ReflexAgent, Minimax, AlphaBeta, Expectimax,
│   │                            # betterEvaluationFunction (phần nhóm cài đặt)
│   ├── autograder.py, layouts/, test_cases/ ...   # bộ khung của môn học
├── BaoCao.pdf / BaoCao.tex
└── README.md
```

Nhóm chỉ chỉnh sửa `search.py`, `searchAgents.py` (Project 1) và `multiAgents.py` (Project 2); các file còn lại là bộ khung của môn học.

## Yêu cầu

- Python 3 (đã chạy thử với Python 3.12 và 3.14), không cần cài thêm thư viện ngoài.
- Chạy các lệnh dưới đây từ trong thư mục chứa `pacman.py`.

## Project 1 – Tìm kiếm

```bash
cd pro1/search/search
```

**Chấm điểm tự động**

```bash
python autograder.py                # toàn bộ q1–q8
python autograder.py -q q7          # một câu
python autograder.py --no-graphics  # không mở cửa sổ đồ họa
```

**Tái hiện số liệu trong báo cáo** (thêm `-q` để chạy không đồ họa)

```bash
# Bảng: DFS / BFS / UCS / A* trên mê cung
python pacman.py -l mediumMaze -p SearchAgent -a fn=dfs -q
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs -q
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs -q
python pacman.py -l mediumMaze -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic -q
python pacman.py -l bigMaze    -p SearchAgent -a fn=bfs -q
python pacman.py -l bigMaze    -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic -q

# Bài toán bốn góc: BFS (không heuristic) và A* với cornersHeuristic
python pacman.py -l mediumCorners -p SearchAgent -a fn=bfs,prob=CornersProblem -q
python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5 -q

# Bài toán ăn hết thức ăn: UCS và A* với foodHeuristic
python pacman.py -l trickySearch -p SearchAgent -a fn=ucs,prob=FoodSearchProblem -q
python pacman.py -l trickySearch -p AStarFoodSearchAgent -q

# Tìm thức ăn gần nhất (tham lam)
python pacman.py -l bigSearch -p ClosestDotSearchAgent -q
```

Bỏ `-q` để xem Pacman chạy trên giao diện đồ họa.

**Kết quả mong đợi**

| Lệnh | Chi phí | Số nút mở rộng |
|---|---|---|
| mediumMaze – DFS | 130 | 146 |
| mediumMaze – BFS / UCS | 68 | 269 |
| mediumMaze – A\* (Manhattan) | 68 | 221 |
| bigMaze – BFS / UCS | 210 | 620 |
| bigMaze – A\* (Manhattan) | 210 | 549 |
| mediumCorners – BFS | 106 | 1966 |
| mediumCorners – A\* (cornersHeuristic) | 106 | 741 |
| trickySearch – UCS | 60 | 16688 |
| trickySearch – A\* (foodHeuristic) | 60 | 4137 |
| bigSearch – ClosestDotSearchAgent | 350 | – |

## Project 2 – Đối kháng

```bash
cd pro2/multiagent/multiagent
```

**Chấm điểm tự động**

```bash
python autograder.py                # toàn bộ q1–q5
python autograder.py -q q2          # một câu
python autograder.py --no-graphics
```

**Chạy từng tác tử** (`-n` là số ván, `-q` chạy không đồ họa)

```bash
python pacman.py -p ReflexAgent    -l openClassic -n 10 -q
python pacman.py -p MinimaxAgent   -l minimaxClassic -a depth=4
python pacman.py -p MinimaxAgent   -l trappedClassic -a depth=3 -q
python pacman.py -p AlphaBetaAgent -l trappedClassic -a depth=3 -n 10 -q
python pacman.py -p ExpectimaxAgent -l trappedClassic -a depth=3 -n 10 -q
python pacman.py -p AlphaBetaAgent -l smallClassic -a depth=3 -n 5 -q

# Hàm đánh giá tốt hơn (dùng ở nút lá của tác tử tìm kiếm)
python pacman.py -p ExpectimaxAgent -l smallClassic -a evalFn=better,depth=2 -n 10 -q
```

**Lưu ý:** hành vi của ma có yếu tố ngẫu nhiên, nên tỉ lệ thắng và điểm trung bình có thể lệch nhẹ giữa các lần chạy (đặc biệt khi số ván ít). Dùng thêm cờ `-f` để cố định seed nếu muốn kết quả lặp lại được. Các kết quả tất định như −501 (Minimax/Alpha-Beta trên trappedClassic) và 516.0 (Minimax độ sâu 4 trên minimaxClassic) luôn giống nhau.

## Tóm tắt phương pháp

- **Khung tìm kiếm đồ thị chung:** DFS, BFS, UCS, A\* chỉ khác nhau ở cấu trúc biên (Stack, Queue, PriorityQueue theo `g`, PriorityQueue theo `g+h`).
- **CornersProblem:** trạng thái là (vị trí, bốn cờ góc đã thăm). Heuristic là tổng khoảng cách Manhattan nhỏ nhất qua mọi hoán vị các góc chưa thăm (chấp nhận được và nhất quán).
- **foodHeuristic:** khoảng cách mê cung thực (BFS) đến viên thức ăn xa nhất, có lưu đệm.
- **Minimax / Alpha-Beta / Expectimax:** dùng chung một hàm đệ quy theo (tác tử, độ sâu); Expectimax coi ma chọn ngẫu nhiên đều.
- **betterEvaluationFunction:** điểm trò chơi + nghịch đảo khoảng cách thức ăn gần nhất − phạt theo số thức ăn/viên năng lượng + thưởng gần ma bị sợ − phạt nặng khi sát ma đang hoạt động.

## Tài liệu tham khảo

1. UC Berkeley CS188 – Introduction to Artificial Intelligence, Pacman projects. <https://ai.berkeley.edu>
2. S. Russell and P. Norvig, *Artificial Intelligence: A Modern Approach*, 4th ed. Pearson, 2020.

## Repository

GitHub: <https://github.com/TEN-TAI-KHOAN/TEN-REPO> *(thay bằng đường dẫn thật)*
