### 项目介绍

- 利用OpenCV图像处理技术对道路图像进行处理，获取车道线位置并进行显示
- 相关功能函数：高斯模糊、Canny边缘检测、Hough变换直线检测

### 处理流程

- 图像转灰度：`cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)`
- 高斯模糊：平滑、降噪
  - `cv2.GaussianBlur(img, (kernel_size, kernel_size), 0)`
- Canny边缘检测：`cv2.Canny(img, low_threshold, high_threshold)`
- 定义感兴趣区域（ROI）：手动限定多边形车道区域
  - `cv2.fillPoly(mask, vertices, ignore_mask_color)`
- ROI内提取车道线边缘：将Canny边缘图像与ROI进行相交，只保留感兴趣区域内的边缘，排除图像中不相关的边缘信息
  - `masked_image = cv2.bitwise_and(img, mask)`
- 霍夫变换检测直线：
  - `lines = cv2.HoughLinesP(img, rho, theta, threshold, np.array([]), minLineLength=min_line_len, maxLineGap=max_line_gap)`
- 车道线原图显示：将检测到的直线与原始图像融合
  - `linesImage = hough_lines(edgeMask, rho, theta, threshold, min_line_len, max_line_gap)`
  - `result = cv2.addWeighted(image, 0.8, linesImage, 1, 0)`