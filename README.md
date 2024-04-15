% Load the image
grayImage = imread('/MATLAB Drive/your_image.mat'); % Replace 'your_image.jpg' with the actual image file path

% Perform edge detection (you can replace this with your preferred edge detection method)
edgeImage = edge(grayImage, 'Canny');

% Get the boundary coordinates using bwboundaries function
boundaries = bwboundaries(edgeImage);

% Extract the first boundary (assuming there's only one object in the image)
boundary = boundaries{1};

% Generate chain code
chainCode = generateChainCode(boundary);

% Display the original image and the detected boundary
figure;
subplot(1, 2, 1);
imshow(image);
title('Original Image');

subplot(1, 2, 2);
imshow(edgeImage);
hold on;
plot(boundary(:, 2), boundary(:, 1), 'r', 'LineWidth', 2);
title('Edge Detection and Chain Code');

% Display the chain code
disp('Chain Code:');
disp(chainCode);

% Function to generate chain code from boundary coordinates
function code = generateChainCode(boundary)
    % Define directions (assuming 8-connectivity)
    directions = [0, 1; -1, 1; -1, 0; -1, -1; 0, -1; 1, -1; 1, 0; 1, 1];

    % Initialize chain code
    code = zeros(1, length(boundary));

    % Convert boundary coordinates to chain code
    for i = 2:length(boundary)
        diffVector = boundary(i, :) - boundary(i - 1, :);
        [~, index] = ismember(diffVector, directions, 'rows');
        code(i) = index - 1; % Adjust for 0-based indexing
    end
end
