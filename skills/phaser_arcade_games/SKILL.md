# Phaser 3 & 2D Arcade Game Expert

## CRITICAL ARCHITECTURAL RULES

### 1. PHASER VERSION & PARTICLES (Phaser 3.60+ / 3.90+)
- NEVER call `particles.createEmitter()` or use `ParticleEmitterManager` (REMOVED in Phaser 3.60+).
- ALWAYS use modern particle emitter syntax:
  ```javascript
  const smoke = this.add.particles(0, 0, 'smoke_particle', {
    speed: { min: 40, max: 80 },
    lifespan: 350,
    scale: { start: 0.6, end: 0 },
    alpha: { start: 0.8, end: 0 },
    blendMode: 'ADD',
    follow: player,
    followOffset: { x: -20, y: 0 }
  });
  ```
- NEVER reuse the player sprite as the particle texture! If you need a particle texture, generate a clean circle or star procedurally using `this.make.graphics()`.

### 2. PROCEDURAL TEXTURE GENERATION (No Missing Assets)
- If external SVG or PNG sprites look crude or fail to load, procedurally generate crisp textures at runtime in `preload()` or `create()`:
  ```javascript
  // Generate smoke particle texture:
  const gSmoke = this.make.graphics({ x: 0, y: 0, add: false });
  gSmoke.fillStyle(0xffaa22, 1);
  gSmoke.fillCircle(6, 6, 6);
  gSmoke.generateTexture('smoke_particle', 12, 12);

  // Generate neon glowing obstacle:
  const gObstacle = this.make.graphics({ x: 0, y: 0, add: false });
  gObstacle.fillStyle(0x00ffcc, 1);
  gObstacle.fillRoundedRect(0, 0, 60, 600, 10);
  gObstacle.lineStyle(2, 0xffffff, 0.8);
  gObstacle.strokeRoundedRect(0, 0, 60, 600, 10);
  gObstacle.generateTexture('neon_pillar', 60, 600);
  ```

### 3. OBSTACLE GAP MATH (Ceiling to Floor)
- Top and bottom obstacles MUST span from the very edges of the screen to leave a central gap.
- Top pillar: Place at `x`, with its bottom at `gapY - gap/2`. Set `origin: (0, 1)` so it extends up into the ceiling.
- Bottom pillar: Place at `x`, with its top at `gapY + gap/2`. Set `origin: (0, 0)` so it extends down to the floor.
- NEVER spawn fixed 200px boxes that float in mid-air leaving gaps at the ceiling or floor.
- Dynamic group: Always use `this.physics.add.group()` (NEVER `staticGroup()` for moving gates).

### 4. ZERO-DEPENDENCY WEB AUDIO SYNTHESIS
- NEVER leave sounds as comments. Generate retro arcade audio procedurally using Web Audio API:
  ```javascript
  let audioCtx = null;
  function getAudioContext() {
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    if (audioCtx.state === 'suspended') audioCtx.resume();
    return audioCtx;
  }

  function playSfx(type) {
    try {
      const ctx = getAudioContext();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.connect(gain).connect(ctx.destination);
      const now = ctx.currentTime;

      if (type === 'flap') {
        osc.frequency.setValueAtTime(340, now);
        osc.frequency.exponentialRampToValueAtTime(620, now + 0.08);
        gain.gain.setValueAtTime(0.2, now);
        gain.gain.linearRampToValueAtTime(0, now + 0.08);
        osc.start(now); osc.stop(now + 0.08);
      } else if (type === 'score') {
        osc.frequency.setValueAtTime(880, now);
        osc.frequency.setValueAtTime(1320, now + 0.06);
        gain.gain.setValueAtTime(0.25, now);
        gain.gain.linearRampToValueAtTime(0, now + 0.14);
        osc.start(now); osc.stop(now + 0.14);
      } else if (type === 'crash') {
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(160, now);
        osc.frequency.exponentialRampToValueAtTime(30, now + 0.25);
        gain.gain.setValueAtTime(0.35, now);
        gain.gain.linearRampToValueAtTime(0, now + 0.25);
        osc.start(now); osc.stop(now + 0.25);
      }
    } catch (e) {}
  }
  ```

### 5. GAME JUICE & DYNAMICS
- **Tilt / Banking**: Tilt nose UP on tap (`player.setAngle(-22)`), and rotate nose DOWN into a dive when falling (`player.angle = Math.min(70, player.angle + 2)` in `update()`).
- **Background Motion**: Add a parallax scrolling starfield or grid background (`tileSprite.tilePositionX += 1.5`).
- **Screens & States**: Include an introductory "TAP TO START" screen, live HUD score, and "GAME OVER" screen with highest score recorded in `localStorage`.

### 6. MOBILE SCALING
- Always set:
  ```javascript
  scale: {
    mode: Phaser.Scale.FIT,
    autoCenter: Phaser.Scale.CENTER_BOTH
  }
  ```
- Listen to `this.input.on('pointerdown')` for seamless touch & mouse operation.

### 7. COMPLETE PRODUCTION BLUEPRINT
```javascript
import Phaser from 'phaser';

let audioCtx = null;
function playSfx(type) {
  try {
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    if (audioCtx.state === 'suspended') audioCtx.resume();
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.connect(gain).connect(audioCtx.destination);
    const t = audioCtx.currentTime;
    if (type === 'flap') {
      osc.frequency.setValueAtTime(340, t);
      osc.frequency.exponentialRampToValueAtTime(620, t + 0.08);
      gain.gain.setValueAtTime(0.2, t);
      gain.gain.linearRampToValueAtTime(0, t + 0.08);
      osc.start(t); osc.stop(t + 0.08);
    } else if (type === 'score') {
      osc.frequency.setValueAtTime(880, t);
      osc.frequency.setValueAtTime(1320, t + 0.06);
      gain.gain.setValueAtTime(0.2, t);
      gain.gain.linearRampToValueAtTime(0, t + 0.14);
      osc.start(t); osc.stop(t + 0.14);
    } else if (type === 'crash') {
      osc.type = 'sawtooth';
      osc.frequency.setValueAtTime(160, t);
      osc.frequency.exponentialRampToValueAtTime(30, t + 0.25);
      gain.gain.setValueAtTime(0.3, t);
      gain.gain.linearRampToValueAtTime(0, t + 0.25);
      osc.start(t); osc.stop(t + 0.25);
    }
  } catch (e) {}
}

export class MainScene extends Phaser.Scene {
  constructor() { super('MainScene'); }

  preload() {
    const gShip = this.make.graphics({ add: false });
    gShip.fillStyle(0x00f0ff, 1);
    gShip.fillTriangle(0, 16, 44, 16, 16, 0);
    gShip.fillStyle(0xff0055, 1);
    gShip.fillTriangle(4, 20, 18, 16, 4, 12);
    gShip.generateTexture('player_jet', 44, 24);

    const gSmoke = this.make.graphics({ add: false });
    gSmoke.fillStyle(0xffaa00, 1);
    gSmoke.fillCircle(5, 5, 5);
    gSmoke.generateTexture('smoke_particle', 10, 10);

    const gPillar = this.make.graphics({ add: false });
    gPillar.fillStyle(0x1a1a2e, 1);
    gPillar.fillRoundedRect(0, 0, 52, 700, 6);
    gPillar.lineStyle(3, 0x00ff88, 1);
    gPillar.strokeRoundedRect(0, 0, 52, 700, 6);
    gPillar.generateTexture('neon_gate', 52, 700);
  }

  create() {
    this.state = 'READY';
    this.score = 0;
    this.highScore = parseInt(localStorage.getItem('pocket_high_score') || '0', 10);
    const { width, height } = this.cameras.main;

    this.bg = this.add.tileSprite(0, 0, width, height, 'smoke_particle')
      .setOrigin(0, 0).setAlpha(0.2).setTint(0x4466aa);

    this.obstacles = this.physics.add.group();

    this.player = this.physics.add.sprite(width * 0.25, height / 2, 'player_jet');
    this.player.setCollideWorldBounds(true);
    this.player.body.setSize(36, 16);
    this.player.body.setGravityY(0);

    this.particles = this.add.particles(0, 0, 'smoke_particle', {
      speed: { min: 40, max: 80 },
      lifespan: 250,
      scale: { start: 0.8, end: 0 },
      blendMode: 'ADD',
      follow: this.player,
      followOffset: { x: -20, y: 0 }
    });

    this.scoreText = this.add.text(width / 2, 40, '0', {
      fontSize: '40px',
      fontStyle: 'bold',
      fontFamily: 'sans-serif',
      color: '#00ffcc'
    }).setOrigin(0.5).setDepth(10);

    this.promptText = this.add.text(width / 2, height * 0.7, 'TAP TO FLY', {
      fontSize: '24px',
      fontStyle: 'bold',
      fontFamily: 'sans-serif',
      color: '#ffffff'
    }).setOrigin(0.5).setDepth(10);

    this.input.on('pointerdown', () => this.handleTap());
    this.physics.add.overlap(this.player, this.obstacles, () => this.handleCrash(), null, this);
  }

  handleTap() {
    if (this.state === 'READY') {
      this.state = 'PLAYING';
      this.promptText.setVisible(false);
      this.player.body.setGravityY(950);
      this.time.addEvent({ delay: 1600, callback: () => this.spawnPillars(), loop: true });
      this.player.setVelocityY(-320);
      this.player.setAngle(-22);
      playSfx('flap');
    } else if (this.state === 'PLAYING') {
      this.player.setVelocityY(-320);
      this.player.setAngle(-22);
      playSfx('flap');
    } else if (this.state === 'GAMEOVER') {
      this.scene.restart();
    }
  }

  spawnPillars() {
    if (this.state !== 'PLAYING') return;
    const { width, height } = this.cameras.main;
    const gap = 160;
    const gapY = Phaser.Math.Between(gap, height - gap);

    const top = this.obstacles.create(width + 40, gapY - gap / 2, 'neon_gate');
    top.setOrigin(0, 1);
    top.setVelocityX(-200);
    top.body.setAllowGravity(false);
    top.body.immovable = true;

    const btm = this.obstacles.create(width + 40, gapY + gap / 2, 'neon_gate');
    btm.setOrigin(0, 0);
    btm.setVelocityX(-200);
    btm.body.setAllowGravity(false);
    btm.body.immovable = true;
    btm.scored = false;
  }

  update(time, delta) {
    if (this.bg) this.bg.tilePositionX += 1.5;
    if (this.state === 'PLAYING') {
      if (this.player.body.velocity.y > 0) {
        this.player.angle = Math.min(65, this.player.angle + 2.2);
      }
      this.obstacles.getChildren().forEach((p) => {
        if (!p.scored && p.x < this.player.x && p.originY === 0) {
          p.scored = true;
          this.score += 1;
          this.scoreText.setText(this.score.toString());
          playSfx('score');
        }
        if (p.x < -60) p.destroy();
      });
    }
  }

  handleCrash() {
    if (this.state !== 'PLAYING') return;
    this.state = 'GAMEOVER';
    playSfx('crash');
    this.physics.pause();
    this.particles.stop();

    if (this.score > this.highScore) {
      this.highScore = this.score;
      localStorage.setItem('pocket_high_score', this.highScore.toString());
    }

    const { width, height } = this.cameras.main;
    this.add.text(width / 2, height * 0.45, 'GAME OVER', {
      fontSize: '44px',
      fontStyle: 'bold',
      color: '#ff2255'
    }).setOrigin(0.5).setDepth(20);

    this.promptText.setText(`BEST: ${this.highScore}  •  TAP TO REPLAY`).setVisible(true).setDepth(20);
  }
}
```