Visual software development tool used to develop video games, animation, high interactive graphical environment

Game engines provide
- level design tools
	- lighting
	- material editor
	- drawing tools
- cinematic (animation tools)
- physics and collision engines
- artificial intelligence
- audio engines
- scene graph (object hierarchy)
- rendering engines in 2D or 3D graphics
- multiplayer features
- programming libraries for designing video games

## Unity vs Unreal Engine
![[Pasted image 20240910095610.png]]

## OpenGL
Learn OpenGL to develop you own game engine or graphics libraries

Advantages:
- Low level
Disadvantages:
- [[OpenGL]] = graphic API, need to write your own libraries for Audio, Physics, collision detection, materials, networking etc
- Not practical to develop high-level game using OpenGL

### Difference between rendering API's and game engines
rendering APIs like OpenGL or DirectX provide just functionality to abstract a graphics accelerator, focusing on rendering primitives, state management, command lists/command buffers
They are different from fully fledged 3D graphics libraries, 3D game engines

Unreal has an abstraction layer graphics rendering API (RHI) above platform dependent APIs such as DirectX 11 (windows), Vulkan(windows), and OpenGL(Linux). Metal (Mac)

Unity uses its own built-in graphics rendering APIs by default, the default graphics API can be changed to DirectX 11 and Vulkan for Windows, OpenGL for Mac and Linux


