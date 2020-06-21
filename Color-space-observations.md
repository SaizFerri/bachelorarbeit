## Color space observations

**Display images with matplotlib**

1. Convert to numpy array
2. Transpose shape to be (W, H, C), where W is the width, H is the Height, and C are the color channels. E.g: (32, 32, 3)

**Normalize RGB image**

`image / 255`

**Unnormalize RGB image**

`image * 255`

**Normalize Lab image**

L channel = `L / 100`
ab channels = `(ab + 128) / 255`

**Unnormalize Lab image**

L channel = `L * 100`
ab channels = `ab * 255 - 128`

**Show grayscale image (L) channel**

1. Convert RGB image to Lab with `rgb2lab()`
2. Initialize matrix with same shape as image with zeros `np.zeros(image.shape)`
3. Set image L values to new matrix `z[:,:,0] = image[:,:,0]`
4. Convert new Lab image to RGB with `lab2rgb()`
5. Display image

**Show color channels from Lab image**

1. Convert RGB image to Lab with `rgb2lab()`
2. Initialize matrix with same shape as image with zeros `np.zeros(image.shape)`
3. Set image L values to 80 for example (we need a lighting value, otherwise image is black) `z[:,:,0] = 80`
4. Set desired color channel to its values, e.g. channel a `z[:,:,1] = image[:,:,1]`
5. Convert new Lab image to RGB with `lab2rgb()`
6. Display image