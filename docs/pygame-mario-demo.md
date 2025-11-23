# 在本機運行簡易「瑪利兄弟風」Pygame 範例

以下步驟示範如何把示例程式碼保存並在本機執行，讓角色可以移動與跳躍。

## 1. 準備環境
- 安裝 Python 3.9 以上版本。
- 建議建立虛擬環境：
  ```bash
  python -m venv .venv
  source .venv/bin/activate  # Windows 請改用 .venv\\Scripts\\activate
  ```
- 安裝 Pygame：
  ```bash
  pip install pygame
  ```

## 2. 保存程式碼
將以下程式碼存成 `mario_demo.py`（可放在任意目錄，例如桌面或專案資料夾）：

```python
import pygame
import sys

pygame.init()
clock = pygame.time.Clock()

SCREEN_WIDTH, SCREEN_HEIGHT = 800, 600
WIN = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Simple Mario-like Game")

PLAYER_WIDTH, PLAYER_HEIGHT = 50, 60
player_x, player_y = 50, SCREEN_HEIGHT - PLAYER_HEIGHT
player_vel_x = 0
player_vel_y = 0
GRAVITY = 0.8
JUMP_VEL = -15

ground_rect = pygame.Rect(0, SCREEN_HEIGHT - 40, SCREEN_WIDTH, 40)
block_rect = pygame.Rect(400, SCREEN_HEIGHT - 80, 80, 40)

def handle_movement(keys):
    global player_x, player_vel_x, player_vel_y
    player_vel_x = 0
    if keys[pygame.K_LEFT]:
        player_vel_x = -5
    if keys[pygame.K_RIGHT]:
        player_vel_x = 5
    if keys[pygame.K_SPACE] or keys[pygame.K_UP]:
        if player_y + PLAYER_HEIGHT >= ground_rect.top:
            player_vel_y = JUMP_VEL

def update_physics():
    global player_x, player_y, player_vel_y
    player_vel_y += GRAVITY
    player_x += player_vel_x
    player_y += player_vel_y

    if player_y + PLAYER_HEIGHT >= ground_rect.top:
        player_y = ground_rect.top - PLAYER_HEIGHT
        player_vel_y = 0

    player_rect = pygame.Rect(player_x, player_y, PLAYER_WIDTH, PLAYER_HEIGHT)
    if player_rect.colliderect(block_rect):
        if player_vel_y > 0 and player_y + PLAYER_HEIGHT - player_vel_y <= block_rect.top:
            player_y = block_rect.top - PLAYER_HEIGHT
            player_vel_y = 0
        else:
            player_x -= player_vel_x

def draw():
    WIN.fill((135, 206, 235))
    pygame.draw.rect(WIN, (0, 200, 0), ground_rect)
    pygame.draw.rect(WIN, (139, 69, 19), block_rect)
    pygame.draw.rect(WIN, (255, 0, 0), (player_x, player_y, PLAYER_WIDTH, PLAYER_HEIGHT))
    pygame.display.update()

def main():
    global player_x, player_y
    while True:
        clock.tick(60)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        keys = pygame.key.get_pressed()
        handle_movement(keys)
        update_physics()
        draw()

if __name__ == "__main__":
    main()
```

## 3. 執行遊戲
在存有 `mario_demo.py` 的目錄開啟終端機，執行：

```bash
python mario_demo.py
```

視窗開啟後，使用 **左右方向鍵** 移動角色，按 **空白鍵** 或 **上方向鍵** 跳躍。碰撞到地面與方塊後會停止下落，能持續操作直到關閉視窗為止。
