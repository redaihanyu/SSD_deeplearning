1、数据处理
===============

=====================
1.1 获取分类数据
=====================

图片数据集截取自大量高清大图，截取的方法类似`labelImg <https://github.com/tzutalin/labelImg>`_。将截取的信息存储在数据库中，每一个小图(即一个样本图片)的相关信息为：filePath，min_x, min_y, w, h，其中filePath表示高清大图路径，其它四个参数表示样本小图的左上角坐标和宽高。最终获取样本小图的代码如下：

.. code-block:: c++
    :linenos:

    Mat imageZoom(string imgFilePath, int roi_min_x, int roi_min_y, int roi_w, int roi_h){
    Mat srcImage = imread(imgFilePath) ;
    srcImage = srcImage(Rect(Point(roi_min_x, roi_min_y), Point(roi_min_x + roi_w, roi_min_y + roi_h))) ;
    int differ_wh_abs = abs(srcImage.cols - srcImage.rows) ;
    
    if(differ_wh_abs > 0){
        int dstImageLength = srcImage.rows > srcImage.cols ? srcImage.rows : srcImage.cols ;
        Mat dstImage = Mat(Size(dstImageLength, dstImageLength), CV_8UC3, Scalar(srcImage.at<Vec3b>(0, 0))) ;
        int extend_length = differ_wh_abs / 2 ;
        Rect mid_rect ;
        
        if(srcImage.rows > srcImage.cols){
            mid_rect = Rect(extend_length, 0, srcImage.cols, srcImage.rows) ;
        }else{
            mid_rect = Rect(0, extend_length, srcImage.cols, srcImage.rows) ;
        }
        srcImage.copyTo(dstImage(mid_rect)) ;
        return dstImage ;
    }else{
        return srcImage ;
    }
}


