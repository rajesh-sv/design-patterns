# Facade Pattern

The Facade Pattern provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsytem of easier to use.

## Diagram

## Example

```python
# Implement Amplifier, CdPlayer, PopcornPopper, Projector, Screen, StreamingPlayer, TheaterLights and Tuner classes

class HomeTheaterFacade:
    amp: Amplifier
    tuner: Tuner
    player: StreamingPlayer
    cd: CdPlayer
    projector: Projector
    lights: TheaterLights
    screen: Screen
    popper: PopcornPopper
        
    def __init__(
        self, 
        amp: Amplifier,
        tuner: Tuner,
        player: StreamingPlayer,
        projector: Projector,
        screen: Screen,
        lights: TheaterLights,
        popper: PopcornPopper
    ):
        self.amp = amp
        self.tuner = tuner
        self.player = player
        self.projector = projector
        self.screen = screen
        self.lights = lights
        self.popper = popper
        
    def watchMovie(self, movie: str) -> None:
        print("Get ready to watch a movie...")
        self.popper.on()
        self.popper.pop()
        self.lights.dim(10)
        self.screen.down()
        self.projector.on()
        self.projector.wide_screen_mode()
        self.amp.on()
        self.amp.set_streaming_player(self.player)
        self.amp.set_surround_sound()
        self.amp.set_volume(5)
        self.player.on()
        self.player.play(movie)
        
    def endMovie(self) -> None:
        print("Shutting movie theater down...")
        self.popper.off()
        self.lights.on()
        self.screen.up()
        self.projector.off()
        self.amp.off()
        self.player.stop()
        self.player.off()
        
    def listenToRadio(self, frequency: float) -> None:
        print("Tuning in the airwaves...")
        self.tuner.on()
        self.tuner.set_frequency(frequency)
        self.amp.on()
        self.amp.set_volume(5)
        self.amp.set_tuner(self.tuner)
        
    def endRadio(self) -> None:
        print("Shutting down the tuner...")
        self.tuner.off()
        self.amp.off()
```
