# Proposal of SCOPE Analysis: Apple Detection Validation Project

Notes: In this project exploratory analysis i use terms of _"projected"_ as the indication for synthetization of data or rough assumptions, only for the project exploratory simulations purposes (contextual awareness and objective alignment), the actual facts and validity might actually far from accurate representative of company

## S: Stakeholder Requirements & Success Criteria

### Primary Stakeholders:

- **Primary Stakeholders**: Company
- **Data Labeling Team**: Requires accurately cropped and pre-classified apple images to be review
- **ML Team**: Elevate Refined data to fine-tune apple classification models

### Business Success Definition:

- Automated pre-classification system with at least 80% accuracy
- Enhancing Manual labelling efficiency

### Error Tolerance:

- **Type I errors (False Positives)**: Medium tolerance
  - _Business Impact_: Incorrect color classification requiring human resource
  - _Mitigation_: Human labeling team managed as a safety net
- **Type II errors (False Negatives)**: Medium tolerance
  - _Business Impact_: Missing apples in the image would reduce dataset size and potential
  - _Mitigation_: Good object detection to ensure all apples are identified

### Interpretability Requirements:

- Clear visualization of cropped apple images with color classification labels
- Confidence scores for each classification to prioritize human review

```
Conclusion of Success Criteria:
1. Maximize apple detection precision and recall overall
2. Achieve reasonable color classification MAP
```

## C: Constraints & Context

### Computational Resources:

- **Training**:
  - Low-performance GPU cluster at Engineer compute station (_projected_)
- **Inference**:
  - Mid or Edge Device at plantation unit probably in remote locations with limited connectivity
  - Not requires of object tracking system as palm tree is a still object, which requires quality of coverage detection rather than fastest inference speed
- **Field Use**: For Laptop or Mobile device that processing aerial images from drone (_projected_)

### Deployment Environment:

- Primary: Cloud-based processing pipeline
- Secondary: Laptop or Mobile Devices for offline processing
- Future consideration:
  - Hybrid Cloud-based for main inference and local Model for backup inference mitigation

### Response Time Requirements:

- Job Processing or minimum delayed inference is still acceptable
- Inference Speed at least of 1 FPS on all target machines

### Data Constraints:

- Apple Characteristics:
  - Various Apple health quality that affect mixing color
  - Various sizes and shapes of apples
  - Different shades of red, yellow, and green
  - Partially visible apples due to occlusion
  - Mixed lighting conditions affecting color perception
- Image Characteristics:
  - Indoor and outdoor photography with varied lighting
  - Different camera qualities and resolutions
  - Potential shadows or reflections affecting color accuracy

## O: Objectives & Optimization Targets

### Primary Metric:

- AP across different class of apple

### Secondary Metrics:

- Recall (AR) across different tree densities (IOU overlapping: 0.5, 0.7, 0.9)
- MAP score for tree detection (balancing precision and recall) since the detection consist of multi class target
- Inference time per image

### Performance Thresholds:

- **Minimum Acceptable**:
  - AP > 70%
  - R > 70%
- **Target**: MAP > 0.7
- **Stretch Goal**: MAP > 0.85

### Acceptable Trade-offs:

- Precision vs. Recall: Prioritize Precision to ensure all apples are detected
- Speed vs. Accuracy: Reasonable processing time (1FPS) with high accuracy

```
Optimization Priority Order:
1. Precision (Detection Quality)
2. Recall (Detection Coverage)
3. Model size efficiency
4. Inference speed
```

## P: Problem Characteristics

### Problem Type:

- Two-stage machine learning task:
  1. Object detection to identify and crop individual apples
  2. Image classification to categorize apples by color (red, yellow, green)

### Data Characteristics:

- **Volume**: Variable number of apples per image (10 typical)
- **Dimensions**: Standard high-resolution photographs
- **Distribution**:
  - Images may contain varying densities of apples, with some images featuring clusters and others having isolated apples.
  - Imbalanced representation of apple colors across the dataset.
  - Potential occlusion from leaves or other apples, complicating detection.

### Special Considerations:

- Including Rotten or Unhealthy apple
- Gradient Color of red, yellow, and green

## E: Evaluation Strategy

### Validation Approach:

### A. Dataset Splitting

- **Training Set**: 90% of the data for training
- **Validation Set**: 5% of the data for hyperparameters tuning.
- **Test Set**: 5% of the data for final evaluation

### B. Cross-Validation

- **K-Fold Cross-Validation**: If the dataset is large enough, use k-fold cross-validation

### Testing Methodology:

### A. A/B Testing

- **Implementation**: Compare the baseline model against alternative model
- **Metrics to Compare**: Use F1, AR, and AP to evaluate performance gap.

### B. Validation

- **Validation**: Conduct manual labeling of palm tree number to validate model predictions

### Monitoring Plan:

### A. Performance Monitoring

- **Real-Time Monitoring**: Set up dashboards to track model performance metrics during training and inference.

### B. Feedback Collection

- **User Feedback**: Regularly gather input from the data labeling team regarding the usability of the interface and accuracy of classifications.
- **Iterative Improvements**: Use feedback and performance metrics to refine the model and retrain with updated datasets.

### Maintenance Strategy:

- **Retraining Schedule**: Schedule the regular model updates to incorporate new data and model improvement regarding to Company computational power

## AI Model Development Approach Selection Framework

## Project Requirements:

1. **Time Constraints**: < 1 Week
2. **Team Expertise**: Software Engineers with some ML or General Developers
3. **Use Case Complexity**: Standard Object Detection (Not for research purpose)
4. **Data Availability**: Medium Dataset (500-1000 images)
5. **Deployment Environment**: :Local deployment, Edge Devices, Mobile Applications, Cloud Services

**Balance development speed vs. performance needs**:

- MMDetection Model Toolbox
- Model API Frameworks (Ultralytics)

Model Tuning Result:

1. Recall: 0.994
2. Precision: 0.899
3. MAP-50: 0.981
4. mAP50-95: 0.793
5. Model size efficiency
6. Inference speed: 2.5ms on Small 640x640 image
