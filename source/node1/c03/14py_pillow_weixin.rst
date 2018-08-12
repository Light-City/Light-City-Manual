.. figure:: http://p20tr36iw.bkt.clouddn.com/py_pillow_weixin.jpg
   :alt: 

.. raw:: html

   <!--more-->

py切割图片微信高逼格朋友圈
==========================

读取图片->填充为正方形->将图像切位9张->保存图片

.. code:: python

    from PIL import Image

    # 先将 input image 填充为正方形
    def fill_image(image):
        # 获取图像的宽和高
        width, height = image.size
        # 获取长和宽中较大值作为新图片
        new_image_length = width if width > height else height
        # 生成新图片
        new_image = Image.new(image.mode, (new_image_length,new_image_length), color="white")
        # 将之前的图粘贴在新图上,居中
        if width > height:
            # 原图宽大于高，则填充图片的竖直维度  #(x,y)二元组表示粘贴上图相对下图的起始位置,是个坐标点。
            new_image.paste(image, (0, int((new_image_length - height) / 2)))
        else:
            new_image.paste(image, (int((new_image_length - width) / 2), 0))
        return new_image

    def cut_image(image):
        width, height = image.size
        item_width = int(width / 3)
        box_list = []
        for i in range(0,3):
            for j in range(0,3):
                box = (j*item_width, i*item_width,(j+1)*item_width,(i+1)*item_width)
                box_list.append(box)
        # crop : 返回图像某个给定区域。box 是一个 4 元素元组，定义了 left, upper, right, lower 像素坐标
        image_list = [image.crop(box) for box in box_list]
        return image_list
    # 保存
    def save_image(image_list):
        index = 1
        for image in image_list:
            image.save('./image/' + str(index) + '.png', 'PNG')
            index += 1

    if __name__ == '__main__':
        file_path = './m.jpg'
        image = Image.open(file_path)
        image = fill_image(image)
        image_list = cut_image(image)
        save_image(image_list)


