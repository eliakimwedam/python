Computer Graphics Practical Examination Study GuideLibrary: Pillow (PIL)Target: Complete Solutions for Practicals 1 to 8 with ExplanationsPractical 1: Creating a Blank Image (Frame Buffer)Question 1Write a Python program using the Pillow library to create a $600 \times 400$ pixel image that represents a frame buffer.
Your program should:Create a blank image with a white background.Display the image.Save the image as frame_buffer.png.
Requirements: Use Image.new(), define width and height using variables, and add comments explaining each step.Code Solutionfrom PIL import Image

def practical_1():
    # Define frame buffer dimensions
    width = 600
    height = 400
    
    # Create a blank white image (RGB mode) representing the frame buffer
    image = Image.new("RGB", (width, height), "white")
    
    # Save the frame buffer to disk
    image.save("frame_buffer.png")
    
    # Display the image in the default viewer
    image.show()

if __name__ == "__main__":
    practical_1()
ExplanationA frame buffer is a video output device memory region that drives a display from a memory buffer containing a complete frame of data. Here, Image.new("RGB", (width, height), "white") instantiates a new digital canvas in RGB color mode with dimensions $600 \times 400$ pixels, initialized entirely to white. The .save() method writes this memory buffer to persistent storage, and .show() launches the local machine's default viewer to inspect the output.Practical 2: Understanding Pixels and CoordinatesQuestion 2Create a Python program that generates a $400 \times 300$ image and plots individual pixels at different coordinates:$(50, 50)$ -> Red$(150, 100)$ -> Blue$(250, 200)$ -> Green$(350, 250)$ -> Black
Requirements: Use putpixel(), RGB color values, and display the final image.Code Solutionfrom PIL import Image

def practical_2():
    width = 400
    height = 300
    
    # Create blank canvas
    image = Image.new("RGB", (width, height), "white")
    
    # Plot individual pixels using (x, y) coordinates and RGB triplets
    image.putpixel((50, 50), (255, 0, 0))       # Red
    image.putpixel((150, 100), (0, 0, 255))     # Blue
    image.putpixel((250, 200), (0, 255, 0))     # Green
    image.putpixel((350, 250), (0, 0, 0))       # Black
    
    image.save("pixel_plot.png")
    image.show()

if __name__ == "__main__":
    practical_2()
ExplanationPixels represent the elementary building blocks of a digital raster image located on a Cartesian grid. The coordinate system starts with $(0, 0)$ at the top-left corner, where $x$ increases horizontally to the right and $y$ increases vertically downward. The .putpixel((x, y), color_tuple) function alters the color of a specific coordinate cell directly in memory.Practical 3: Creating Visible Pixels Using Pixel BlocksQuestion 3A single pixel is too small to observe. Write a Python program that creates large visible pixels:Create a $400 \times 300$ image.Create a function called draw_pixel().Allow the function to draw a pixel block of size $20 \times 20$.Draw a red pixel at $(50, 50)$, blue at $(150, 100)$, and green at $(250, 150)$.Code Solutionfrom PIL import Image

def draw_pixel(image, x, y, color, size=20):
    """Draws a scaled visible pixel block of size (size x size) at coordinate (x, y)."""
    for i in range(size):
        for j in range(size):
            image.putpixel((x + i, y + j), color)

def practical_3():
    image = Image.new("RGB", (400, 300), "white")
    
    # Draw magnified pixel blocks
    draw_pixel(image, 50, 50, (255, 0, 0), size=20)    # Red block
    draw_pixel(image, 150, 100, (0, 0, 255), size=20)  # Blue block
    draw_pixel(image, 250, 150, (0, 255, 0), size=20)  # Green block
    
    image.save("pixel_blocks.png")
    image.show()

if __name__ == "__main__":
    practical_3()
ExplanationBecause modern screens have ultra-high pixel densities (PPI), a single coordinate pixel is often microscopic. By designing a helper function draw_pixel() with nested loops over a size scalar ($20 \times 20$), we create a macro-block of adjacent pixels sharing the same color value, making individual pixels easily visible to the human eye.Practical 4: Drawing Lines Using PixelsQuestion 4Using individual pixels, create a program that draws:A. Horizontal Line: $(50, 100)$ to $(350, 100)$ - RedB. Vertical Line: $(200, 50)$ to $(200, 250)$ - BlueC. Diagonal Line: $(50, 50)$ to $(250, 250)$ - Green
Requirement: Do not use ImageDraw.line(); manually plot the pixels.Code Solutionfrom PIL import Image

def practical_4():
    image = Image.new("RGB", (500, 400), "white")
    
    # A. Horizontal Line: vary x while y remains constant
    for x in range(50, 351):
        image.putpixel((x, 100), (255, 0, 0))
        
    # B. Vertical Line: vary y while x remains constant
    for y in range(50, 251):
        image.putpixel((200, y), (0, 0, 255))
        
    # C. Diagonal Line: increment x and y simultaneously (slope = 1)
    for i in range(201):
        image.putpixel((50 + i, 50 + i), (0, 255, 0))
        
    image.save("manual_lines.png")
    image.show()

if __name__ == "__main__":
    practical_4()
ExplanationRasterizing lines manually teaches fundamental graphics algorithms (like DDA or Bresenham's line algorithm). A horizontal line keeps $y$ fixed while iterating through a range of $x$ values. A vertical line keeps $x$ fixed while iterating through $y$. A diagonal line with a $45^\circ$ angle increments both coordinates simultaneously by adding an offset loop variable $i$ to both starting coordinates.Practical 5: Drawing Rectangular Borders Using PixelsQuestion 5Develop a Python program that creates rectangles using individual pixels:Rectangle 1: $(50, 50)$ to $(200, 150)$ - RedRectangle 2: $(250, 100)$ to $(400, 250)$ - BlueRequirements: Use loops, putpixel(), and draw only the borders.Code Solutionfrom PIL import Image

def draw_rect_border(img, start_x, start_y, end_x, end_y, color):
    # Draw top and bottom horizontal boundaries
    for x in range(start_x, end_x + 1):
        img.putpixel((x, start_y), color)
        img.putpixel((x, end_y), color)
    # Draw left and right vertical boundaries
    for y in range(start_y, end_y + 1):
        img.putpixel((start_x, y), color)
        img.putpixel((end_x, y), color)

def practical_5():
    image = Image.new("RGB", (500, 400), "white")
    
    # Draw Rectangle 1 (Red)
    draw_rect_border(image, 50, 50, 200, 150, (255, 0, 0))
    
    # Draw Rectangle 2 (Blue)
    draw_rect_border(image, 250, 100, 400, 250, (0, 0, 255))
    
    image.save("rectangle_borders.png")
    image.show()

if __name__ == "__main__":
    practical_5()
ExplanationAn unfilled geometric rectangle consists of four distinct line segments: top, bottom, left, and right. By leveraging loops across the minimum and maximum boundary coordinates ($x_{start}$ to $x_{end}$ and $y_{start}$ to $y_{end}$), we plot pixels exclusively along the perimeter outlines without filling the inner interior space.Practical 6: Creating Filled Shapes Using PixelsQuestion 6Write a Python program that creates a filled rectangle using pixels.Start at coordinate: $(100, 100)$End at coordinate: $(300, 250)$Fill color: RGB $(255, 165, 0)$ (Orange)Requirements: Use nested loops and putpixel().Code Solutionfrom PIL import Image

def practical_6():
    image = Image.new("RGB", (500, 400), "white")
    
    # Nested loops iterate through every coordinate inside the bounding box
    for x in range(100, 301):
        for y in range(100, 251):
            image.putpixel((x, y), (255, 165, 0)) # Orange fill
            
    image.save("filled_rectangle.png")
    image.show()

if __name__ == "__main__":
    practical_6()
ExplanationTo fill a 2D shape, we must color every single pixel bound within the interior area. Nested loops achieve this by having an outer loop iterate across every horizontal column ($x$) and an inner loop iterate vertically down each row ($y$) for every column, ensuring complete area coverage.Practical 7: Exploring RGB Color ModelsQuestion 7Create a Python program that demonstrates the RGB color model by creating three distinct color sections where intensities increase from $0 \rightarrow 255$:Section 1: Red intensity gradientSection 2: Green intensity gradientSection 3: Blue intensity gradientRequirement: Display the final color gradient image.Code Solutionfrom PIL import Image

def practical_7():
    image = Image.new("RGB", (600, 300), "white")
    
    # Section 1: Red gradient (Width: 0 to 200)
    for x in range(200):
        intensity = int((x / 200) * 255)
        for y in range(300):
            image.putpixel((x, y), (intensity, 0, 0))
            
    # Section 2: Green gradient (Width: 200 to 400)
    for x in range(200, 400):
        intensity = int(((x - 200) / 200) * 255)
        for y in range(300):
            image.putpixel((x, y), (0, intensity, 0))
            
    # Section 3: Blue gradient (Width: 400 to 600)
    for x in range(400, 600):
        intensity = int(((x - 400) / 200) * 255)
        for y in range(300):
            image.putpixel((x, y), (0, 0, intensity))
            
    image.save("rgb_color_model.png")
    image.show()

if __name__ == "__main__":
    practical_7()
ExplanationThe RGB color model uses three channels (Red, Green, Blue) each ranging from $0$ (complete absence of light) to $255$ (maximum brightness). This script maps horizontal progress across subsections of the canvas to a proportional scaling factor, dynamically calculating color channel intensity to generate smooth linear color ramps.Final Practical Project: Pixel Art Design Using Python PillowQuestion 8: Creating the AMFEX Logo Using PixelsUsing only pixels, squares, rectangles, loops, and RGB colors, create a pixel-based design displaying the word AMFEX.
Requirements:Create a canvas of $800 \times 400$ pixels.Use small square blocks as pixels.Design each letter manually using matrix bitmap patterns or block offsets.Code Solutionfrom PIL import Image

def draw_pixel_block(image, start_x, start_y, matrix, color, scale=6):
    """Draws a custom character using a 2D matrix bitmap pattern and a modular scale factor."""
    for row_idx, row in enumerate(matrix):
        for col_idx, val in enumerate(row):
            if val == 1:
                # Render a scaled square block for each active matrix bit
                for i in range(scale):
                    for j in range(scale):
                        px = start_x + (col_idx * scale) + j
                        py = start_y + (row_idx * scale) + i
                        if px < image.width and py < image.height:
                            image.putpixel((px, py), color)

def practical_8():
    # Canvas size: 800 x 400 pixels with a dark studio background
    image = Image.new("RGB", (800, 400), (20, 24, 33))
    
    scale = 6 # Pixel scaling module size
    
    # 5x5 Bitmap templates for AMFEX (1 = filled, 0 = background space)
    letter_A = [
        [0, 1, 1, 1, 0],
        [1, 0, 0, 0, 1],
        [1, 1, 1, 1, 1],
        [1, 0, 0, 0, 1],
        [1, 0, 0, 0, 1]
    ]
    
    letter_M = [
        [1, 0, 0, 0, 1],
        [1, 1, 0, 1, 1],
        [1, 0, 1, 0, 1],
        [1, 0, 0, 0, 1],
        [1, 0, 0, 0, 1]
    ]
    
    letter_F = [
        [1, 1, 1, 1, 1],
        [1, 0, 0, 0, 0],
        [1, 1, 1, 0, 0],
        [1, 0, 0, 0, 0],
        [1, 0, 0, 0, 0]
    ]
    
    letter_E = [
        [1, 1, 1, 1, 1],
        [1, 0, 0, 0, 0],
        [1, 1, 1, 1, 0],
        [1, 0, 0, 0, 0],
        [1, 1, 1, 1, 1]
    ]
    
    letter_X = [
        [1, 0, 0, 0, 1],
        [1, 0, 0, 0, 1],
        [0, 1, 1, 1, 0],
        [1, 0, 0, 0, 1],
        [1, 0, 0, 0, 1]
    ]
    
    start_y = 150
    
    # Render letters across the canvas with distinct modern RGB colors
    draw_pixel_block(image, start_x=80,  start_y=start_y, matrix=letter_A, color=(255, 75, 75),   scale=scale) # Crimson Red
    draw_pixel_block(image, start_x=220, start_y=start_y, matrix=letter_M, color=(75, 150, 255),  scale=scale) # Vivid Blue
    draw_pixel_block(image, start_x=360, start_y=start_y, matrix=letter_F, color=(75, 255, 125),  scale=scale) # Neon Green
    draw_pixel_block(image, start_x=500, start_y=start_y, matrix=letter_E, color=(255, 200, 50),  scale=scale) # Gold Yellow
    draw_pixel_block(image, start_x=640, start_y=start_y, matrix=letter_X, color=(200, 75, 255),  scale=scale) # Purple
    
    image.save("amfex_logo.png")
    image.show()

if __name__ == "__main__":
    practical_8()
ExplanationBitmap typography uses 2D arrays (matrices of $1$s and $0$s) to define font glyphs. Here, each character is mapped out in a $5 \times 5$ grid. The draw_pixel_block function iterates through the matrix rows and columns, drawing scaled pixel blocks (scale=6) wherever a $1$ appears. This demonstrates how raster graphics engines render custom vector/bitmap typography using basic loop iteration and pixel plotting.
