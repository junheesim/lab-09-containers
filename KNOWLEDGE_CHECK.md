# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.

This is a bad idea because 1. the credentials will be permanently accessible by anyone who can pull the image (even if you delete them with a later instruction), and 2. when the Learner Lab session ends (after 4 hours), the credentials will simply not work and break the environment.

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

When the container's environment variables were found not to contain the MongoDB credentials, the SSM parameter set in AWS was accessed using boto3 by mounting the host directory for the AWS credentials on my machine into the container through -v.

## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?

Before: 6364050083b7
After: 31c3809f78a4

This is a problem as the same tag of 0.1.0 really points to a different image, and now there would be multiple versions of the same tag which another person on the team may find difficult to differentiate between if the only documented identifier is the image tag. What should've been done is the tag itself should be changed to another number to make this difference clearer.
