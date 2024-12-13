The datasets provided for vehicle damage detection are well-structured with a training, testing, and evaluation image folder for each category, along with an annotation file in COCO format. However, I have noticed some challenges, such as the annotations containing invalid boxes where a few bounding boxes had zero width or height, which were handled during preprocessing (see section...). Another challenge was that some annotations were empty, which has also been addressed during implementation. Lastly, the bounding boxes were provided as x, y, width, height and transformed into x_min, y_min, x_max, y_max.

## Documentation:
For the implementation of this project, we had the choice to use two different models, and we decided to use Faster RCNN. My choice was primarily based on the efficiency of the model, as well as its availability to use pretrained weights, which accelerates convergence and improves performance. YOLO is faster but does not promise high accuracy.
Due to the limited time for this assignment, I chose not to use data augmentation but still applied some transformations, such as resizing and normalization, to standardize the input.
For the preprocessing of the data, I handled the challenges mentioned in the following way:
To handle invalid boxes, such as bounding boxes with zero or null width/height, I implemented a filter that checks for these issues and ensures that only valid boxes are passed to the model, preventing errors during training.
For empty annotations (images with no bounding boxes), I decided to treat them similarly to invalid boxes by assigning empty tensors for both boxes and labels. These will be processed in the same way as invalid boxes, ensuring that the model can handle such cases without errors. Additionally, some images had varying numbers of bounding boxes and labels. To address this, I padded the annotations with zeros to match the maximum number of boxes in a batch. This ensures that each image has the same number of boxes, allowing for consistent input during training.
I implemented a custom collation function for the dataloader. The function is called collate_fn and dynamically manages preprocessing. It stacks padded bounding boxes, labels, and images into tensors, and includes a check for NaN values in the tensors, allowing me to debug and ensure the integrity of the data at every step.

## Model Architecture: What was your model of choice for tackling this problem and why?
The chosen architecture for this task was Faster R-CNN with a ResNet-50 FPN backbone, selected for its ability to enhance performance on objects at different scales through the Feature Pyramid Network (FPN), provide high-quality region proposals for accurate bounding box predictions using the Region Proposal Network (RPN), and leverage pretrained weights on COCO to speed up training and improve overall performance.

## Design Choices: 
Design choices included implementing a custom collate_fn to batch variable-length targets and handle padding, filtering out invalid boxes during preprocessing to ensure compatibility with the model's input format, and using accuracy, precision, recall, and F1 score for evaluation to provide a comprehensive understanding of model performance.

Accuracy: 0.68
Precision: 0.45
Recall: 0.26
F1 Score: 0.27

The discrepancies betwen the classes (see the picture joint in the zip file called 'distribution') explain the low precision recall as i didn't handle the classes imbalanced.
Some samples of damage predictions and actuals has also been joint to the file.
