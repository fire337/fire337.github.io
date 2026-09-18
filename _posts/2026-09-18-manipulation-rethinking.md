---
layout: post
title: "Rethinking Embodied Manipulation Setup from First Principles"
date: 2026-09-18 00:00:00
---

# Background
Recently generalization and few-shot learning ability emerged through scaling pretraining, such as GEN-1.5[^1], Dyna-2[^2], ACT-2[^3], S1[^4]. There is a clear tendency towards egocentric scaling, video understanding, tactile and agentic manipulation[^5]. Before we dive into the scaling procedure, let's hold on a second and think about a key question: is it a good way to scale up base on the current setup of embodied manipulation?

# Rethinking from First Principles
Humans can perform dexterous manipulation tasks fast and smooth, they can perceive, plan and interact with various objects. Common setup of a manipulation robot contains head view camera, wrist view camera, robotic arm with joint motors, gripper or dexterous hand as end effector and VLA model as brain.   

Wrist camera, which was early used in teleoperation back to 1990s[^6], provides clear and additional view for manipulator. Wrist camera is important to current manipulation task: robot can't see objects clearly from head view because of occlusion or the lack of image details. Additionally, VLA models learn to predict action sequence relative to wrist pose and it is much easier with wrist view as a direct reference.    

However humans can manipulate without an eye in their hand, they can memorize and reconstruct an object's appearance in their mind through different view over time. This benefit from common knowledge and visual memory.   

Wrist camera is important in current manipulation task but it's not the best for the future. Three following aspects(visual memory, dual head view and tactile sensor) can be discussed to take place of wrist camera.

## Visual Memory
Long manipulation tasks rely on context and observations are expanded through visual memory. However context is expensive in terms of visual memory. LLM can handle 1M context because language is highly compressed and informative while vision is not. We should control long visual memory under a fix computation budget during online inference. Here are serval methods to deal with it.

### Long Short Memories
Atkinson & Shiffrin[^7] proposed long-short memory in 1968 from cognitive psychology and lots of work[^8] used that idea in video understanding. Long-term memory can be compressed as memory token or summarized as textual memory.

### Streaming Memory
Streaming memory is designed to continuously process incoming visual information while keeping constant computation[^9]. Same idea can be found in other aspect such as LSTM[^10] and Mamba[^11]. Streaming memory is more compatible with online inference.

### H264-like Memory
Inter-frame images have little variance in pixel space, it's redundant to encode them without compression. Video compression methods such as h264 might be a good direction to explore. StreamFlow[^13] filters temporal redundancy before visual encoding. Feng[^14] and FVC[^15] learn feature level residuals.

### Goal Attention Memory
We can also prune visual tokens base on the manipulation instruction. SparseVLM[^12] use goal text tokens to rate the significance of visual tokens using self-attention matrices and then prune visual tokens significantly.

### TestTimeTraining Memory
TTT[^16] update the model parameters from testing distribution before making a prediction. It might be an efficient way to represent long visual memory.

## Dual Head View
While observations are expanded through visual memory, object details are insufficient without wrist camera. Increasing resolution of head view is a good way to fix this problem. Humans can process high resolution visual input because they tend to focus on ROI(region of interest) and ignore unconcerned parts. We can retrieve further details from high resolution head camera inspired by humans. Two head cameras with dual view can also help understand depth and reconstruct 3D object better, making manipulation more precise. 

## Tactile Sensor
It is difficult for vision to capture contact with an object as their is slightly visual difference. Tactility  give more feedback and act as a good complement to vision, we don't need a wrist camera to capture contact details any more. Imagine a scene, you can lace your shoe with your eyes closed. You achieve this through daily practice and tactility. 

# New Embodied Manipulation Setup
As discussed above, new setup of a manipulation robot should contain high resolution head camera, robotic arm with joint motors, dexterous end effector with tactile sensor and VLA model with visual memory. Wrist camera is removed and the new setup is more compatible with human.   
Large amount of work will be done in data scaling, we should go in a way right way from the beginning of scaling.

# Looking Forward
Think twice from the first principle is important. I am glad if you are interested in the topics above and any discussion is welcomed through X, gamil or github. I'm also looking for research cooperation.

# Reference

[^1]: [GEN-1.5: Embodied Foundation Models are One-Shot Learners](https://generalistai.com/blog/gen-1.5)

[^2]: [Training Dyna-2 at million-hour scale, repeatably](https://www.dyna.co/research/dyna-2-infrastructure)

[^3]: [ACT-2 Preview: Generalizing Reliability](https://www.sunday.ai/blog/act-2-preview)

[^4]: [Introducing S1: In-Context Learning for Robotics](https://skild.ai/blogs/s1)

[^5]: [GPT‑6 Astra on robotic manipulation](https://openai.robocurve.org/gpt-6-astra/)

[^6]: [Wrist Camera Orientation for Effective Telerobotic Orbital Replaceable Unit (ORU) Changeout](https://ntrs.nasa.gov/citations/19970036322)

[^7]: [Atkinson, R. C., & Shiffrin, R. M. (1968). Human memory: A proposed system and its control processes]

[^8]: [MovieChat: From Dense Token to Sparse Memory for Long Video Understanding](https://arxiv.org/abs/2307.16449)

[^9]: [StreamingVLM: Real-Time Understanding for Infinite Video Streams](https://arxiv.org/abs/2510.09608)

[^10]: [Hochreiter, S., & Schmidhuber, J. (1997). Long Short-Term Memory. Neural Computation, 9(8), 1735–1780.]

[^11]: [Mamba: Linear-Time Sequence Modeling with Selective State Spaces](https://arxiv.org/abs/2312.00752)

[^12]: [SparseVLM: Visual Token Sparsification for Efficient Vision-Language Model Inference](https://arxiv.org/abs/2410.04417)

[^13]: [StreamFlow: Dynamic Memory Flows for Streaming Video Understanding](https://arxiv.org/abs/2608.10949)

[^14]: [Learned Video Compression with Feature-level Residuals](https://arxiv.org/abs/2004.08283)

[^15]: [FVC: A New Framework towards Deep Video Compression in Feature Space](https://arxiv.org/abs/2105.09600)

[^16]: [Test-Time Training with Self-Supervision for Generalization under Distribution Shifts](https://arxiv.org/abs/1909.13231)
