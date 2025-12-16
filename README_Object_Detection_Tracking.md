# Object Detection & Tracking with Azure Computer Vision

[![Azure](https://img.shields.io/badge/Azure-Video%20Indexer-0089D6.svg)](https://azure.microsoft.com/en-us/services/video-indexer/)
[![Azure](https://img.shields.io/badge/Azure-Custom%20Vision-purple.svg)](https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/)
[![Computer Vision](https://img.shields.io/badge/Task-Object%20Tracking-green.svg)]()
[![Level](https://img.shields.io/badge/Level-Distinction-gold.svg)]()

## Repository Info

**Repo Name:** `azure-object-detection-tracking`

**Description:** Object detection and tracking implementation using Azure Computer Vision, Custom Vision, and Video Indexer. Features bounding box detection, multi-object tracking across video frames, movement visualization, and performance evaluation. Includes demo video. Distinction-level MSc project.

**Topics:** `object-detection` `object-tracking` `azure-cognitive-services` `azure-video-indexer` `custom-vision` `bounding-box` `computer-vision` `deep-learning` `video-analysis` `movement-tracking`

---

## Overview

This project demonstrates practical implementation of object detection and tracking using Azure's Computer Vision services. It includes static image analysis with bounding boxes, video-based object tracking, movement path visualization, and comprehensive performance evaluation.

## Project Highlights

| Component | Implementation |
|-----------|----------------|
| **Image Detection** | Azure Vision Studio - Dense Captions |
| **Video Tracking** | Azure Video Indexer |
| **Objects Tracked** | Dog (single), Children (multiple) |
| **Visualization** | Movement path plotting with coordinates |
| **Evaluation** | Accuracy, limitations, improvements |

## Implementation Pipeline

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Upload     │────▶│   Detect     │────▶│    Track     │────▶│  Visualize   │
│   Media      │     │   Objects    │     │   Movement   │     │    Paths     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
   Image/Video        Bounding Boxes       Frame-by-Frame       Position Plot
                      + Labels             Identity Persist      + Analysis
```

## Part 1: Object Detection in Images

### Azure Setup
1. Created Video Indexer Resource in Azure Portal
2. Configured resource group: `ObjectDetectionRG`
3. Region: East US 2
4. Connected to Vision Studio for image analysis

### Detection Results

**Test Image:** Still life with multiple objects (purse, sunglasses, geometric shapes)

**Detected Objects with Bounding Boxes:**

| Object | Classification | Spatial Description |
|--------|---------------|---------------------|
| 1 | Purse/Handbag | On table surface |
| 2 | Sunglasses | On green surface |
| 3 | Yellow round object | Top of green surface |
| 4 | Red flower | Close-up detection |
| 5 | Green square | Next to leather surface |
| 6 | Green rectangular object | Close-up |
| 7 | Rock | Close-up |
| 8 | Red block | Close-up |

**Dense Captioning Output:**
- "A purse and sunglasses on a green background"
- "A yellow round object on top of a green surface"
- "A close-up of a red flower"
- "A handbag and sunglasses on a table"

### Key Observations
- System generates **contextual descriptions**, not just labels
- Identifies **spatial relationships** between objects
- Provides **color recognition** and **shape identification**
- JSON output includes confidence scores and exact bounding box coordinates

## Part 2: Object Tracking Across Video Frames

### Video 1: Single Object Tracking (Dog)

**Source:** 7-second clip - Dog walking along shoreline at sunset

**Tracking Performance:**
| Metric | Result |
|--------|--------|
| Labels Detected | outdoor, animal, water, dog breed, sky |
| Scenes Identified | 1 |
| Keyframes Generated | 3 |
| Identity Persistence | ✅ Maintained throughout |

**Movement Analysis:**
- Consistent left-to-right movement along shoreline
- Maintained tracking despite changing posture/orientation
- Successfully handled reflective water background
- Gait pattern and stride detected across frames

### Video 2: Multiple Object Tracking (Children)

**Source:** 22-second clip - Two children sitting on grass with statue

**Tracking Performance:**
| Metric | Result |
|--------|--------|
| Labels Detected | clothing, person, outdoor, tree, human face, grass |
| Scenes Identified | 1 |
| Shots Identified | 1 (2 keyframes) |
| Objects Tracked | 2 (separate identities maintained) |

**Observations:**
- Both children tracked as **separate entities**
- Static elements (statue, trees) correctly identified
- Environmental context properly integrated

## Part 3: Movement Visualization

### Position Tracking Plot

```
Y Position
    │
1500┤     ●────────────────────────────────●
    │   Person 1 (stationary)          Person 2 (moving)
1400┤
    │
1300┤
    │
1200┤  ●━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━●
    │  Start                              End
1100┤
    └────────────────────────────────────────
        1600    1800    2000    2200    2400    2600
                        X Position
```

**Movement Data:**
- **Person 1 (Blue):** Coordinates ~(1505, 1205) - Minimal movement
- **Person 2 (Red):** Moved from (1505, 1200) to (2600, 1200) - Significant horizontal displacement

### Dog Movement Path

```
Movement Pattern: Left → Right along shoreline
Elevation: Consistent (parallel to water line)
Pace: Steady across all frames
Bounding boxes: Adjusted dynamically to dog's changing posture
```

## Part 4: Performance Evaluation

### Strengths

| Capability | Evidence |
|------------|----------|
| **Identity Persistence** | Dog maintained identity across all frames despite movement |
| **Multi-Object Handling** | Two children tracked separately without confusion |
| **Environmental Context** | Correctly identified settings (outdoor, water, grass) |
| **Temporal Consistency** | Appropriate keyframe generation (3 for 7-sec video) |
| **Spatial Tracking** | Accurate position logging for visualization |

### Limitations

| Limitation | Example |
|------------|---------|
| **Classification Granularity** | Generic "animal" instead of specific dog breed |
| **Contextual Misinterpretation** | Children's white clothing labeled as "wedding dress" |
| **Processing Latency** | Several minutes for 5-10 second videos |
| **Frame Sampling** | Keyframes only, may miss rapid movements |
| **API Accessibility** | Programmatic access challenging with credentials |

### Accuracy Assessment

| Aspect | Rating | Notes |
|--------|--------|-------|
| Primary Object Detection | ⭐⭐⭐⭐⭐ | Excellent for main subjects |
| Fine-grained Classification | ⭐⭐⭐ | Generic categories preferred |
| Identity Tracking | ⭐⭐⭐⭐⭐ | No identity switches observed |
| Environmental Context | ⭐⭐⭐⭐ | Good scene understanding |
| Processing Speed | ⭐⭐⭐ | Noticeable latency |

## Part 5: Recommended Improvements

Based on research evidence:

### 1. Advanced Tracking Algorithms
> Zhang et al. (2021): "DeepSORT reduced identity switches by 45% compared to traditional tracking methods in crowded scenes" (p. 732)

**Application:** Incorporate appearance features with motion prediction for better occlusion handling.

### 2. Domain-Specific Fine-Tuning
> Cui et al. (2023): "Domain-specific fine-tuning led to 32% reduction in misclassification errors for specialized object categories" (p. 217)

**Application:** Train models for specific contexts (wildlife, retail, medical) to improve classification accuracy.

### 3. Environmental Robustness
> Liu et al. (2022): "Models trained with diverse weather conditions maintained 91% of their performance in adverse environments, compared to only 63% for standard models" (p. 892)

**Application:** Data augmentation for lighting variations, weather conditions, and camera shake.

## Technical Architecture

### Azure Services Used

```
┌─────────────────────────────────────────────────────────┐
│                    Azure Portal                          │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐ │
│  │   Vision    │  │   Custom    │  │     Video       │ │
│  │   Studio    │  │   Vision    │  │    Indexer      │ │
│  └──────┬──────┘  └──────┬──────┘  └────────┬────────┘ │
│         │                │                   │          │
│         ▼                ▼                   ▼          │
│  ┌─────────────────────────────────────────────────┐   │
│  │              Cognitive Services API              │   │
│  │  • Object Detection    • Dense Captioning        │   │
│  │  • Bounding Boxes      • Scene Analysis          │   │
│  │  • Label Generation    • Keyframe Extraction     │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Processing Pipeline

1. **Upload** → Azure Storage Account
2. **Analyze** → Computer Vision API / Video Indexer
3. **Extract** → Bounding boxes, labels, keyframes
4. **Track** → Identity persistence across frames
5. **Visualize** → Position plots, movement paths
6. **Evaluate** → Accuracy metrics, limitations

## Demo Video

🎥 **[Azure Custom Vision Demo](demo/)** (3 minutes)

Contents:
- Introduction to Azure Custom Vision
- Frame-by-frame object detection demonstration
- Dog tracking across video sequence
- Bounding box visualization
- Key observations and challenges
- Applications: wildlife monitoring, retail, manufacturing, security

## Real-World Applications

| Domain | Application |
|--------|-------------|
| **Security** | Surveillance monitoring, intruder detection |
| **Retail** | Customer tracking, inventory management |
| **Wildlife** | Animal behavior monitoring |
| **Manufacturing** | Quality control, defect detection |
| **Sports** | Player tracking, movement analysis |
| **Healthcare** | Patient monitoring, fall detection |

## Repository Structure

```
azure-object-detection-tracking/
├── README.md
├── docs/
│   ├── Task_5_Submission.pdf
│   └── Task_5_D.pdf
├── demo/
│   └── Azure_Custom_Vision_Demo.mp4
├── images/
│   ├── object_detection_results.png
│   ├── bounding_boxes.png
│   ├── video_tracking.png
│   └── movement_visualization.png
└── results/
    └── tracking_coordinates.json
```

## References

- Cui, Y., et al. (2023). Specialized visual models through domain-specific fine-tuning. *IEEE Trans. Image Processing*, 32(1), 208-221.
- Liu, Q., et al. (2022). Enhancing object detection performance in challenging environmental conditions. *IEEE Trans. Image Processing*, 31(4), 881-895.
- Zhang, L., et al. (2021). Advances in multi-object tracking: A comprehensive analysis. *IEEE TPAMI*, 43(3), 721-737.

## About

**Author:** Victor Prefa, MD  
**Program:** Master of Data Science & Business Analytics, Deakin University (Distinction)  
**Course: Engineering AI Solutions  
**Level:** Distinction-level submission

## Clinical Relevance

Object tracking has significant healthcare applications: patient monitoring in hospitals, fall detection for elderly care, surgical instrument tracking, and rehabilitation movement analysis. My clinical background informs how these technologies can be practically deployed in medical settings.

## License

Educational project. Available as reference for Azure Computer Vision implementations.
