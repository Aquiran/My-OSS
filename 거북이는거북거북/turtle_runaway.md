## 1. 타이머 만들기

start함수가 실행될 때, 시작 시간을 저장할 변수 
start_time를 만들고 시작 시간을 저장한다.

```python
    self.start_time = time.time()
```

그 후, step함수가 실행될 때, 이전 결과를 undo함수로 지우고 penup함수로 거북이가 그림을 그리지 못하도록 만든다. 마지막으로, (200, 300) 위치에 (현재시간 - 시작 시간)으로 게임 시간을 구하고 이를 정수로 출력한다.

```python
    self.drawer.undo()
    self.drawer.penup()
    self.drawer.setpos(200, 300)
    self.drawer.write(f'Score: {self.score} Time: {int(time.time() - self.start_time)}')
```

## 2. 점수판 만들기

start함수가 실행될 때, 점수를 저장할 변수
score를 만들고 0으로 초기화한다.

```python
    self.score = 0
```

그 후, step함수가 실행될 때, is_catched가 True라면 score를 1씩 증가시킨다. 그리고 score를 화면에 출력한다. 

```python
    if is_catched:
        self.score += 1
    self.drawer.undo()
    self.drawer.penup()
    self.drawer.setpos(200, 300)
    self.drawer.write(f'Score: {self.score} Time: {int(time.time() - self.start_time)}')
```

## 3. 도망자 거북이 AI 만들기

도망자 거북이(우리가 조종하지 않는 거북이)의 run_ai함수에 도망자 거북이의 위치를 저장할 변수 x, y와 추격자 거북이(우리가 조종하는 거북이)의 위치를 저장할 변수 ox, oy를 만든다. 두 거북이의 위치의 차이를 저장할 dx, dy변수를 만든다.

```python
    def run_ai(self, opp_pos, opp_heading):
            x, y = self.pos()
            ox, oy = opp_pos
            dx, dy = x - ox, y - oy
```

만약, 두 거북이 사이의 거리 차이가 20000보다 작을 경우, 도망자 거북이 math.degrees(math.atan2(dy, dx))로 두 거북이 사이의 각도를 계산하고 + 180을 더해 반대로 이동하게 한다. 여기에 랜덤성을 더하기위해 -30 ~ +30까지의 랜덤한 각도를 추가한다. 만약, 두 거북이 사이의 거리가 멀면 랜덤하게 움직이도록 만든다. 

```python
    if dx**2 + dy**2 < 20000:
        self.setheading(math.degrees(math.atan2(dy, dx)) + 180 + random.randint(-30, 30))
        self.forward(self.step_move)
    else:
        mode = random.randint(0, 2)
        if mode == 0:
            self.forward(self.step_move)
        elif mode == 1:
            self.left(self.step_turn)
        elif mode == 2:
            self.right(self.step_turn)
```