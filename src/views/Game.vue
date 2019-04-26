<template>
  <div id="game"></div>
</template>

<script>
    import Phaser from 'phaser'
    import wall from '../assets/wall.png'
    import ground from '../assets/ground.png'
    import player from '../assets/player.png'
    export default {
        name: 'Game',
        mounted () {
            // PHASER 2.6 -> phaser-ce
            // PHASER 3.0 -> phaser
            let config = {
                type: Phaser.AUTO,
                width: 500,
                height: 200,
                physics: {
                    default: 'arcade',
                    arcade: {
                        gravity: { y: 200 }
                    }
                },
                // NO HI HA STATES A 3.0 -> SCENES
                scene: {
                    preload () {
                        console.log('PRELOAD')
                        // CARREGAR ASSETS -> IMAGES, PLAYERS (SPRITESHEETS), AUDIOS, VIDEOS
                        this.load.image('wall', wall)
                        this.load.image('ground', ground)
                        this.load.spritesheet('player', player, { frameWidth: 28, frameHeight: 22 })
                        // AUDIO
                        // this.load.setBaseURL('http://labs.phaser.io')
                        // this.load.audio('dust', ['assets/dead.wav', 'assets/dead.mp3'])
                    },
                    create () {
                        // INITILIZE del nivell -> Afegirem tiles (level: pareds, terras), afegirem player/s, collectibles (coins) , enemies
                        console.log('CREATED')
                        this.cameras.main.backgroundColor.setTo(52, 152, 219)
                        // GROUP PER DEFECTE ES DINAMIC
                        // this.level = this.physics.add.group()
                        this.level = this.physics.add.staticGroup()
                        // this.add.image(500 / 2 - 160, 200 / 2, 'wall')
                        // this.add.image(500 / 2 + 160, 200 / 2, 'wall')
                        // this.ground = this.physics.add.image(500 / 2, 200 / 2 + 30, 'ground')
                        this.level.create(500 / 2 - 160, 200 / 2, 'wall')
                        this.level.create(500 / 2 + 160, 200 / 2, 'wall')
                        this.level.create(500 / 2, 200 / 2 + 30, 'ground')
                        // this.ground.body.allowGravity = false
                        // PLAYER -> FÍSIQUES
                        this.player = this.physics.add.sprite(500 / 2, 200 / 2 - 50, 'player')
                        this.player.setBounce(0.2)
                        // this.player.setCollideWorldBounds(true)
                        this.physics.add.collider(this.player, this.level)
                        // DEFINE SOUNDS
                        // this.dustSound = this.add.audio('dust', 0.1)
                        // cursors
                        this.cursors = this.input.keyboard.createCursorKeys()
                        // ANIMATE PLAYER
                        this.anims.create({
                            key: 'idle',
                            frames: this.anims.generateFrameNumbers('player', { start: 3, end: 5 }),
                            frameRate: 5,
                            repeat: -1
                        })
                    },
                    update () {
                        this.player.anims.play('idle', true)
                        // INPUT EVENTS
                        if (this.cursors.left.isDown) {
                            this.player.setVelocityX(-160)
                            this.player.setFrame(2)
                        } else if (this.cursors.right.isDown) {
                            this.player.setVelocityX(160)
                            this.player.setFrame(1)
                        } else {
                            this.player.setVelocityX(0)
                        }
                        // JUMP
                        if (this.cursors.up.isDown && this.player.body.touching.down) {
                            this.player.setVelocityY(-160)
                        }
                        // MULTIPLE JUMP (JUMP ON THE AIR)
                        // if (this.cursors.up.isDown) {
                        //   this.player.setVelocityY(-160)
                        // }
                        // console.log('UPDATED')
                        // ESTE EL QUE S'executa continuament al Game loop -> 60 vegades per segon o FPS
                        // if (this.player.body.touching.down && this.player.y > 10) {
                        //   this.dustSound.play()
                        // }
                    }
                }
            }
            // eslint-disable-next-line
            new Phaser.Game(config)
        }
    }
</script>