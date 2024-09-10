# Triangle-detection-using-Hough-Transform

How to run the code:

  Make sure that before running the last 4 cells, that the path of the images from images folder is correct.
  For the rest of the code, it should run without any problem.

  
Here is our approach:
  1. For each image we chose a window size of (100,100), and step size to right and down are 20,40 respectively. (except for sandwiches image we chose a window of size (310,180) and only made steps to the right.
     
  2. For each window we found the Hough transform by using the edge map of the current window. We filtered out weak peaks by using maximum filtering of a neighbourhood of size (100,100).This helped us not choosing redundant lines. Then we did non maximum supperssion to remove redundant lines that maximum filtering couldn't remove. By that we got all the lines which are candidates to be a part of sides of triangles.
  3. To identify triangles we checked every combination of 3 lines that forms a triangle. for every combination we did the following:
       1. we got the intersection of the 3 lines, which forms 3 points.
       2. each two point forms a line that is a candidate to be a triangle side. For the 3 lines we get we checked if their length is greater than some threshold, if yes this means that these 3 lines can be as sides in a triangle. else we move to the other combination.
       3. given that the 3 lines are greater than the threshold, means that they form a triangle, let's call it triangle X. We define a mask of size of edge image. and we fill the pixels that represent triangle X in mask by a nonzero value. Till now we got mask which represent triangle X, now we do bitwise AND between the mask and the edge image, if triangle X is really a triangle in the input image, the resut of the bitwise and should be that the majority of the nonzero pixels in the mask before the bitwise operation are still non zero pixels after the operation. for that we chose a threshold, if the result is greter than threshold, means that is X is a triangle in the input image.
       4. But before using the edge image in step 3 we blurred it by using Gaussian Blur, to get more accurate results.
       5. After we got all the triangles now we must classify them to equilateral, isosceles, right isosceles or right triangles. To identify:
            1. equilateral triangles: we computed the sides of the triangles by calculating the Eculidean distance between vertices of the triangle. if all of them are equal then it is equilateral.
            2. isosceles/ right isosceles triangles: calculate the Eculidean distance between vertices of the triangle, if at least two sides are equal then it is isosceles. to check if they are right isosceles, we ordered the sides based on length, and applied pythagoras.
            3. right isosceles: we ordered the sides based on length, and applied pythagoras.
       6. To plot the results:
            1. We ploted the detected peaks on hough transform, and the detected lines on the edge map for each window that we found in it a line that belong to a triangle. we did that because as we mentioned earlier we only do sliding window to detect lines not triangles.
            2. We used the lines of the triangle to Hough transfrom and plot the peaks of classified triangles each with it's color-coded color.
            3. the same with lines. for triangles which are non of the types above we drawn it using white color.(as an indication that we also detected them.)
            4. Also, the same with detected trinagles.
         
               
Overall our code is generic for every input image. But before, we need to pick the correct parameters for each image. But over all it works almost for every image.  

