# Concept generation
This page introduces 3 concepts as solutions to the problem statement, including sketch:it, triangle:bit and bit:board.

## sketch:it
sketch:it attempts to include drawing into the world of electronics, and this is made possible by the fact that graphite conducts electricity. It has been proven that micro:bit reacts to hand-drawn buttons using graphite pencils, which make use of capacitive touch sensing. sketch:it makes use of this property; users can draw their own buttons, hence creating their own controller.

![sketch for sketch:it](https://raw.githubusercontent.com/t-tht/bitboard-docs/master/Images/sketchit3.jpg)

### Physical features
sketch:it includes fixed inputs: 4 touch sensors and a joystick. micro:bit is connected to the sketch:it via the edge connector to use the fixed inputs. If the user opts to use hand-drawn buttons, then they can simply insert the micro:bit into the space next to the edge connector. The user draws the buttons on paper and slide it into the slot at the bottom of the sketch:it. With micro:bit inserted into the space, connection is established between the graphite tracks and the edge of micro:bit.

## triangle:bit
triangle:bit takes its inspiration from Scrabble Flash from Hasbro Games, which consists of 5 electronic blocks; each block shows a letter and blocks are put together to make words. triangle:bit works on a similar idea, where blocks of inputs are put together along with the main block according to the user's liking to form different shapes.

![sketches for triangle:bit](https://raw.githubusercontent.com/t-tht/bitboard-docs/master/Images/trianglebit.jpg)
![sketches for triangle:bit](https://raw.githubusercontent.com/t-tht/bitboard-docs/master/Images/trianglebit2.jpg)
![sketches for triangle:bit](https://raw.githubusercontent.com/t-tht/bitboard-docs/master/Images/trianglebit3.jpg)

As seen from above, this design allows flexibility in shapes of the product, making customization easy.

### Blocks
Three types of blocks are considered: input blocks, intermediate blocks and the main block.
- Input blocks: Inputs including joystick, touch sensors and dial
- Intermediate blocks: provides connection between input blocks and the main block
- The main block: holds the micro:bit

## bit:board
bit:board - takes its name from clipboard - includes a canvas and a set of fixed inputs(joystick and 4 touch sensors). bit:board unleashes user's imagination and explores the potential of micro:bit. Users can draw their own buttons - similar to sketch:it - but could also make use of the fixed inputs. The difference between bit:board and sketch:it is that bit:board is more oriented to developing an understanding of electronic circuits; bit:board makes pins of micro:bit more accessible, giving more flexibility in input creation.

![sketches for bit:board](https://raw.githubusercontent.com/t-tht/bitboard-docs/master/Images/bitboard.jpg)  

### Canvas
The canvas should ideally take the size of A5, and held down by clips which is hard-wired to pins of micro:bit and should also allow electrical connection to realize any 'components' (i.e. buttons or potentiometers) drawn.

### Change of input modes
bit:board allows 3 input modes, and can be changed via crocodile clips:
- Fixed inputs: allows user to make use to the fixed inputs only(i.e. joystick and 4 touch sensors)
- Customized inputs: users can create their own inputs to micro:bit via different means of conducting material such as graphite pencils or copper tape
- Mixed inputs: users could make use of both fixed inputs and customized inputs at the same time, depending on pin arrangement of the micro:bit
