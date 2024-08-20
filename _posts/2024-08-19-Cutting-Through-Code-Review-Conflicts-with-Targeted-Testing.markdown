---
layout: post
title:  "Cutting Through Code Review Conflicts with Targeted Testing"
date:   2024-08-18 16:37:21 +0300
categories: SoftwareEngineering
tags: SoftwareEngineering testing CodeReview
image: /assets/2024-08-19-Cutting-Through-Code-Review-Conflicts-with-Targeted-Testing/developers_in_conflict.jpeg
---

![]({{ page.image }})

### Introduction

Code reviews are an essential part of the software development process, but they can sometimes become battlegrounds. One common issue is disagreements over potential bugs or edge cases. These disputes can be particularly challenging when the reviewee believes their code is correct, while the reviewer suspects otherwise.

When such disagreements arise, they often lead to unproductive discussions that can strain team relationships. Instead of finding a resolution, the conversation might turn into a back-and-forth argument. This not only slows down the development process but can also create a tense atmosphere within the team.

To address these conflicts efficiently, I’ve developed a simple trick: writing targeted tests. This approach helps to objectively determine whether the code handles specific scenarios correctly, cutting through subjective arguments and leading to a clearer resolution.

### A Personal Experience: When a Code Review Turned into a Debate

I recall a particularly challenging code review where a colleague and I disagreed on how a piece of code handled edge cases. I had put significant effort into ensuring the robustness of the implementation. However, my reviewer pointed out a potential issue with handling a specific input scenario that I had missed.

What began as a straightforward review quickly escalated into a lengthy debate. My initial reaction was to defend my code, insisting that the edge case was already handled. Conversely, the reviewer was equally convinced that there was a potential bug, leading to a prolonged and unproductive discussion.

Rather than continuing the back-and-forth, I suggested implementing a targeted test case specifically for the scenario in question. This seemed like the most efficient way to settle the disagreement and get a definitive answer.

The test results revealed that the reviewer’s concern was valid—the code failed under the specific condition. Although it was difficult to acknowledge my mistake, the targeted test prevented a potential bug from reaching production and ultimately saved us from future headaches.

![First rule of code reiew](/assets/2024-08-19-Cutting-Through-Code-Review-Conflicts-with-Targeted-Testing/code_review_funny.png)

### The Simple Trick: Implementing a Targeted Test

The core idea is to write a specific test case that directly addresses the concern raised during the review. By doing so, you can objectively determine whether the issue exists or not, rather than relying on subjective opinions.

After discovering a bug in my own code through this technique, I’ve adopted it in my role as a reviewer as well. When I encounter something unclear and suspect a potential bug, and the reviewee insists that I’m missing something, I now aim to cut the conversation short and suggest implementing a targeted test. 
This method shifts the discussion from opinions and assumptions to objective results. A well-written test case can clearly show whether the code handles the scenario correctly, making it easier to resolve conflicts and reach consensus.

While writing additional tests may slightly increase review time, the benefits far outweigh this minor trade-off. The enhanced clarity, improved code quality, and more collaborative review process make it a worthwhile investment, while also contributing to a more positive and constructive team atmosphere.


### Are Targeted Tests Too Specific?

One potential criticism of this approach is that these targeted tests can be very specific and closely tied to the private implementation details of the code. Some might argue that such tests don’t contribute to the overall robustness of the codebase and could even be seen as clutter.

While it’s true that these tests are often narrowly focused, I believe the benefits outweigh the potential downsides. First, these tests serve a crucial role in catching bugs that might otherwise slip through the cracks, ensuring that the code behaves as expected in edge cases. Second, they help maintain a positive review process by providing an objective resolution to disagreements, preventing prolonged debates that can lead to frustration and tension.

In my experience, these specific tests don’t harm the codebase. In fact, they often enhance it by adding clarity and confidence to the implementation. The peace of mind that comes from knowing your code has been thoroughly tested, especially in areas where potential issues have been flagged, is invaluable. 

### Conclusion

Disagreements during code reviews are almost inevitable, especially when potential bugs or edge cases are involved. These conflicts, while common, can lead to prolonged discussions. However, by implementing a simple yet effective strategy — **writing targeted tests** — you can transform these debates into productive, objective discussions.

This approach not only helps in quickly resolving whether a bug truly exists but also enhances the overall quality of the codebase. Moreover, it promotes a healthier team dynamic by shifting the focus from personal opinions to objective results. As someone who has experienced the benefits of this technique both as a reviewee and a reviewer, I can confidently say that it’s a valuable tool for anyone involved in code reviews.

I encourage you to try this method in your next code review. **Now, all you need to do is convince the reviewee to write that additional test!**
