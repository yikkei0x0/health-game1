![遊戲封面](icon.png)
  # 洛克人衛生射擊 - 健康守護戰

import pygame
import random
import sys
import os

pygame.init()

  WIDTH, HEIGHT = 800, 600
  screen = pygame.display.set_mode((WIDTH, HEIGHT))
  pygame.display.set_caption("洛克人衛生射擊-健康守護戰")

  # === 設定視窗圖示 ===
  icon_path = "icon_png"
  if os.path.exists(icon_path):
  icon = pygame.image.load(icon_path)
  pygmae.display.set_icon(icon)

  clock = pygame.time.Clock()

  # 顏色 (備用)
  WHITE = (255, 255,255)
  BLACK = (0, 0, 0)
  RED = (0 , 0, 0)
  GREEN = (0, 255, 0)
  BLUE = (0, 100, 255)

 # ====================== 載入圖片 ======================
def load_image(filename, default_size, color=BLUE):
    path = os.path.join("assets", filename)
    if os.path.exists(path):
        try:
            img = pygame.image.load(path).convert_alpha()
            return pygame.transform.scale(img, default_size)
        except:
            pass
    # 找不到圖片就用預設色塊
    surf = pygame.Surface(default_size)
    surf.fill(color)
    return surf

# 載入圖示
player_img = load_image("player.png", (40, 50), BLUE)
enemy_img = load_image("enemy.png", (35, 35), RED)
bullet_img = load_image("bullet.png", (20, 8), (255, 220, 0))

class Player(pygame.sprite.Sprite):
    def _init_(self):
        super()._init_()
        self.image = player_img
        self.rect = self.image.get_rect()
        self.rect.x = 100
        self.rect.y = HEIGHT // 2
        self.speed = 8
        self.vel_y = 0

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_UP] or keys[pygame.K_w]:
            self.rect.y -= self.speed
        if keys[pygame.K_DOWN] or keys[pygame.K_s]:
            self.rect.y += self.speed

        self.vel_y += 0.8
        self.rect.y += self.vel_y

        if self.rect.top < 50: 
            self.rect.top = 50
        if self.rect.bottom > HEIGHT - 50:
            self.rect.bottom = HEIGHT - 50
            self.vel_y = 0

    def shoot(self):
        bullet = Bullet(self.rect.right, self.rect.centery)
        all_sprites.add(bullet)
        bullets.add(bullet)

class Bullet(pygame.sprite.Sprite):
    def _init_(self, x, y):
        super()._init_()
        self.image = bullet_img
        self.rect = self.image.get_rect(center=(x, y))
        self.speed = 15

    def update(self):
        self.rect.x += self.speed
        if self.rect.left > WIDTH:
            self.kill()

class Enemy(pygame.sprite.Sprite):
    def _init_(self):
        super()._init_()
        self.image = enemy_img
        self.rect = self.image.get_rect()
        self.rect.x = WIDTH + random.randint(0, 100)
        self.rect.y = random.randint(80, HEIGHT - 80)
        self.speed = random.randint(4, 7)

    def update(self):
        self.rect.x -= self.speed
        if self.rect.right < 0:
            self.kill()

# ====================== 題庫（20題） ======================
questions = [ ... ]   # 保持原本的20題題庫（可自行擴充）

# ====================== 遊戲初始化 ======================
player = Player()
all_sprites = pygame.sprite.Group()
all_sprites.add(player)
bullets = pygame.sprite.Group()
enemies = pygame.sprite.Group()

lives = 3
score = 0
game_over = False
asking_question = False
current_question = None
selected = 0

# ====================== 其餘程式碼（draw_text、show_question、主迴圈）======================
# ...（與上一版幾乎相同，只需替換 class 定義即可）

# 在主迴圈中 all_sprites.draw(screen) 會自動使用圖片
