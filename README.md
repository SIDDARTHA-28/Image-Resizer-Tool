# Image-Resizer-Tool
Resize and convert images in batch.
import os
import argparse
from PIL import Image

def resize_images(input_folder, output_folder, size, output_format):
    # Create output folder if it doesn't exist
    os.makedirs(output_folder, exist_ok=True)

    for filename in os.listdir(input_folder):
        if filename.lower().endswith(('.png', '.jpg', '.jpeg', '.bmp', '.gif', '.tiff')):
            img_path = os.path.join(input_folder, filename)
            img = Image.open(img_path)

            # Resize image
            img_resized = img.resize(size, Image.ANTIALIAS)

            # Change file extension based on chosen format
            base_name = os.path.splitext(filename)[0]
            new_filename = f"{base_name}.{output_format.lower()}"
            save_path = os.path.join(output_folder, new_filename)

            # Save resized image
            img_resized.save(save_path, output_format)

            print(f"Processed: {filename} → {new_filename}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Batch Image Resizer Tool")
    parser.add_argument("input_folder", help="Path to input folder containing images")
    parser.add_argument("output_folder", help="Path to save resized images")
    parser.add_argument("--width", type=int, default=800, help="Width of resized image (default=800)")
    parser.add_argument("--height", type=int, default=800, help="Height of resized image (default=800)")
    parser.add_argument("--format", type=str, default="JPEG", help="Output format (JPEG, PNG, etc.)")

    args = parser.parse_args()
    size = (args.width, args.height)

    resize_images(args.input_folder, args.output_folder, size, args.format.upper())
pip install pillow
python image_resizer.py input_images output_images --width 600 --height 600 --format PNG
