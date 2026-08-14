from PIL import Image

# ------------------------------------------------------------
# 1. DDA Line Drawing (floating point)
# ------------------------------------------------------------
def dda_line(img, x0, y0, x1, y1, color):
    """Draw a line using Digital Differential Analyzer (DDA)."""
    dx = x1 - x0
    dy = y1 - y0
    steps = max(abs(dx), abs(dy))
    if steps == 0:
        img.putpixel((x0, y0), color)
        return
    x_inc = dx / steps
    y_inc = dy / steps
    x, y = x0, y0
    for _ in range(steps + 1):
        img.putpixel((round(x), round(y)), color)
        x += x_inc
        y += y_inc

# ------------------------------------------------------------
# 2. Bresenham Line Drawing (integer, all octants)
# ------------------------------------------------------------
def bresenham_line(img, x0, y0, x1, y1, color):
    """Draw a line using Bresenham's integer algorithm."""
    dx = abs(x1 - x0)
    dy = abs(y1 - y0)
    sx = 1 if x0 < x1 else -1
    sy = 1 if y0 < y1 else -1
    err = dx - dy

    while True:
        img.putpixel((x0, y0), color)
        if x0 == x1 and y0 == y1:
            break
        e2 = 2 * err
        if e2 > -dy:
            err -= dy
            x0 += sx
        if e2 < dx:
            err += dx
            y0 += sy

# ------------------------------------------------------------
# 3. Bresenham Circle (midpoint algorithm)
# ------------------------------------------------------------
def bresenham_circle(img, cx, cy, radius, color):
    """Draw a circle using the midpoint (Bresenham) algorithm."""
    x = 0
    y = radius
    d = 3 - 2 * radius  # initial decision parameter

    def draw_circle_points(cx, cy, x, y):
        """Plot the eight symmetric points."""
        img.putpixel((cx + x, cy + y), color)
        img.putpixel((cx - x, cy + y), color)
        img.putpixel((cx + x, cy - y), color)
        img.putpixel((cx - x, cy - y), color)
        img.putpixel((cx + y, cy + x), color)
        img.putpixel((cx - y, cy + x), color)
        img.putpixel((cx + y, cy - x), color)
        img.putpixel((cx - y, cy - x), color)

    while x <= y:
        draw_circle_points(cx, cy, x, y)
        if d < 0:
            d += 4 * x + 6
        else:
            d += 4 * (x - y) + 10
            y -= 1
        x += 1

# ------------------------------------------------------------
# 4. Main script
# ------------------------------------------------------------
def main():
    # Create a 500x500 dark gray background (64, 64, 64)
    width = 500
    height = 500
    img = Image.new("RGB", (width, height), (64, 64, 64))

    # (a) White DDA line from (100, 400) to (400, 100)
    dda_line(img, 100, 400, 400, 100, (255, 255, 255))

    # (b) White DDA line from (100, 100) to (400, 400) → X shape
    dda_line(img, 100, 100, 400, 400, (255, 255, 255))

    # (c) Red Bresenham circle centered at (250, 250) radius 80
    bresenham_circle(img, 250, 250, 80, (255, 0, 0))

    # (d) Green Bresenham horizontal line from (0, 250) to (500, 250)
    bresenham_line(img, 0, 250, 500, 250, (0, 255, 0))

    # (e) Blue filled rectangle at (50, 50) width 60 height 40
    rect_x = 50
    rect_y = 50
    rect_w = 60
    rect_h = 40
    for y in range(rect_y, rect_y + rect_h):
        for x in range(rect_x, rect_x + rect_w):
            # Ensure we stay inside the image boundaries (though here they are)
            if 0 <= x < width and 0 <= y < height:
                img.putpixel((x, y), (0, 0, 255))

    # Save the result
    img.save("q3_combined.png")
    print("Image saved as q3_combined.png")

if __name__ == "__main__":
    main()
