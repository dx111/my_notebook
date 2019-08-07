# 第一个笔记本

```c++
#include <opencv2/opencv.hpp>


int main() {

	std::vector<cv::String> file_names;

	cv::glob("images/", file_names);
	
	int cnt = 0;
	for (auto it = file_names.begin(); it != file_names.end(); ++it) {
		cv::Mat src_image, hsv_image, maskimg;

		src_image = cv::imread(*it);

		cv::cvtColor(src_image, hsv_image, CV_BGR2HSV);
		
		cv::inRange(hsv_image, cv::Scalar(0, 43, 46), cv::Scalar(34, 255, 255), maskimg);

		std::vector<std::vector<cv::Point>> contours;
		
		cv::findContours(maskimg, contours, CV_RETR_EXTERNAL, CV_CHAIN_APPROX_NONE);

		for (int i = 0; i < contours.size(); i++) {

			float area = cv::contourArea(contours[i]);

			cv::Scalar color;

			if (area < 100.0) {
				color = cv::Scalar(255, 0, 0);
			}
			else{
				color = cv::Scalar(0, 255, 0);
				cv::Rect rect = cv::boundingRect(contours[i]);
				cv::rectangle(src_image, rect, color,3);
			}

			//cv::drawContours(src_image, contours, i, color,-1);
		}
		
		cv::imwrite("res/"+std::to_string(cnt) + ".jpg",src_image);
		cnt++;
		//
	}

	return 0;
}
```

