% ================ COIN IDENTIFICATION ====================
%      By   : Singgih Bekti W
%      Email: singbekti@gmail.com
%      IG   : @singgih.bekti
%      Note : 
%================ COIN IDENTIFICATION ====================

clc; clear; close all; warning off all; %clear all variables from the current workspace
 
% 1. Read original image from the directory
% read image from the directory
path_directory=('C:\Users\Singgih\OneDrive - Chulalongkorn University\SMT 2\2102642 Computer Vision and Video Electronics\Exam Take Home\Coin\Coin_Take Home Exam _Project\');
path_imagename=('1.JPG');
path_file=strcat(path_directory,path_imagename); %combine path between directory and file as string
img1=imread(path_file);
Img = img1; % re-defining the image
figure, imshow(Img); % showing the original image

% 2. Convert rgb image color space to grayscale
Img_gray = rgb2gray(Img); % convert original image to gray image
figure, imshow(Img_gray); % showing the converted image (gray image)

% 3. Perform image segmentation
% converting grayscale image to binary
bw = im2bw(Img_gray,graythresh(Img_gray));
% completes the image so that the object is one and the background is zero
bw = imcomplement(bw);
figure, imshow(bw); %showing the converted image (binary)

% 4. Perform morphological operations to improve segmentation results
% morphological operations to enhance the segmentation of the opening area to remove noise
bw = bwareaopen(bw,100);
% filling holes --> to fill in the hollow object
bw = imfill(bw,'holes');
% closing --> to make the object shape more smooth
str = strel('disk',5); 
bw = imclose(bw,str);
% remove objects attached to the border (image edge)
bw = imclearborder(bw);
figure, imshow(bw); %showing the segmented image

% 5. Identify each coin based on the color characteristics (in the YCbCr color space) and the specified size (object area)

% labeling of each object
[B,L] = bwlabel(bw);
% calculate the area and centroid of the object
stats = regionprops(B,'All');
% convert rgb image to YCbCr
YCbCr = rgb2ycbcr(Img);
% extract the Cb component (Chrominance-blue)
Cb = YCbCr(:,:,2);
% showing the YCbCr image
figure, imshow(YCbCr)
% showing rgb image (original image)
figure, imshow(Img);
hold on
% create a boundary for the detected coins
Boundaries = bwboundaries(bw,'noholes');

% 6. Calculating information and defining coins
% in this case, for n = 1 and/to n = number of objects
for n = 1:L
    % defining the boundary
    boundary = Boundaries{n};
    % calculate the Cb value of each object
    bw_label = (B==n);
    %defining the label in each object based on Cb
    Cb_label = immultiply(Cb,bw_label);
    Cb_label = (sum(sum(Cb_label)))/(sum(sum(bw_label)));
    
    % calculate the area and centroid of each object
    Area = stats(n).Area; % area for n-object
    centroid = stats(n).Centroid; % centroid for n-object
    
    % defining centers for all
    centers = stats.Centroid; 
    % obtaining diameters for each object
    diameters = mean([stats.MajorAxisLength stats.MinorAxisLength],2);
    % obtaining the radius for each object
    radii = diameters/2;
        % comparing the Area for defining the coins based on the observation
        % nested if for performing the coins
        
        % if the "Area" is more that 11990 --> hence recognized as 0.25 coins <--> (11921-11980)
        if Area<=11990 nilai = 0.25; 
		elseif Area>=15100 && Area<=15300   nilai= 0.5; %(15100-15300)
        elseif Area>=18600 && Area<=18800   nilai= 1;  %(18674-18709)
        elseif Area>=22800 && Area<=24000   nilai= 2;  %(22908-23033)
        elseif Area>=26700 && Area<=27021  nilai= 5;  %(26782-26879)
        else
            nilai = 10; % for other definition = 10 baht coin 
        end
        % displays the boundary on the object
        plot(boundary(:,2), boundary(:,1), 'c', 'LineWidth', 4)
        % displays the coin value on the object centroid
        text(centroid(1)-50,centroid(2),num2str(nilai),...
            'Color','c','FontSize',20,'FontWeight','bold');
end
