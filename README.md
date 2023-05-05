## [中文文档](./README-zh.md)
#  Stable Diffusion webui Infinite Image Browsing
## Key Features

### Excellent Performance
- Once caching is generated, images can be displayed in just a few milliseconds.
- Images are displayed by default using thumbnails with a default size of 256 pixels. You can adjust the size of the thumbnails in the global settings page.
- Extensive optimizations have been made for image preview. However, it is important to make sure that each file has a unique file path. It is recommended to use a format with the created time as the file name.[See FAQ](https://github.com/zanllp/sd-webui-infinite-image-browsing/issues/90)

### 'Walk' Mode
- Browse images automatically by loading the next folder `(similar to os.walk)`, allowing you to browse all images without paging.
- Tested to work properly with over 27,000 files.

### Image Search & Favorite
- Supports image search using Prompt, Model, Lora, etc.
- Tags will be listed based on their frequency of use.
- Supports tag autocomplete, translation, and customization.
- Image favorite can be achieved by toggling custom tags for images in the right-click menu.

### View Image & `Send To`
- Supports viewing image generation information.
- Supports sending images to other tabs.
- Supports full-screen preview.
- Supports moving images in full-screen preview mode.

### Standalone Operation
- Supports standalone operation without sd-webui.
- Almost all functions can be used normally.
- [Click here for details](https://github.com/zanllp/sd-webui-infinite-image-browsing/issues/47).

### Preview based on File Tree Structure & File operations
- Supports preview based on the file tree structure.
- Supports basic file operations as well as multi-select deletion/moving.
- Press and hold Ctrl, Shift, or Cmd to select multiple items.
- Supports sending files directly to other folders via context menu.


It is strongly recommended to use "Open in new tab" (in the upper right corner of the plugin startup page), which is much more comfortable than being embedded in Gradio. However, the "send image to other tab" function cannot be used in this mode.


## Preview

<img width="1920" alt="image" src="https://user-images.githubusercontent.com/25872019/232167682-67f83b00-4391-4394-a7f6-6e4c9d11f252.png">

## Image Search

During the first use, you need to click and wait for the index generation. For my case with 20,000 images, it took about 15 seconds (with an AMD 5600X CPU and PCIe SSD). For subsequent uses, it will check whether there are changes in the folder, and if so, it needs to regenerate the index. Usually, this process is very fast.

Image search supports translation, and you need to place a "tags-translate.csv" file in the plugin folder. You can find this file in the issue . Feel free to share files for other languages to facilitate everyone's use.

<img width="1024" alt="image" src="https://user-images.githubusercontent.com/25872019/234639132-0240dad0-86aa-499b-8e00-20bf2d7f06c3.png">
<img width="620" alt="image" src="https://user-images.githubusercontent.com/25872019/234639759-2d270fe5-b24b-4542-b75a-a025ba78ec89.png">

## Full Screen Preview

<img width="1024" alt="image" src="https://user-images.githubusercontent.com/25872019/232167416-32a8b19d-b766-4f98-88f6-a1d48eaebec0.png">

In full-screen preview mode, you can also view image information and perform operations on the context menu. It supports dragging, resizing and expanding/collapsing .

https://user-images.githubusercontent.com/25872019/235327735-bfb50ea7-7682-4e50-b303-38159456e527.mp4

### Right-click menu
<img width="536" alt="image" src="https://user-images.githubusercontent.com/25872019/232162244-e728d510-b6c6-45e6-afb3-872bd67db05b.png">

### Walk mode


https://user-images.githubusercontent.com/25872019/230768207-daab786b-d4ab-489f-ba6a-e9656bd530b8.mp4




### Dark mode

<img width="768" alt="image" src="https://user-images.githubusercontent.com/25872019/230064879-c95866ac-999d-4d4b-87ea-3e38c8479415.png">
