# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Jakub Stach

```
The initial population is 1000 individuals subdivided into 200 at weddings and 800 at brunches as in the blogpost. The author then uses np.random.choice to randomly select 10% of the individuals to be infected (i.e. the attack rate), they then use np.random.rand to randomly trace the infected based on trace_success which has a 20% success rate - this shows. They then do use value_counts and conditional indexing to do trigger secondary contact tracing The stages are infecting individuals, primary contact tracing, secondary contact tracing, and then simulating different iterations of the test.

More detailed explanation of the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post:

1. Initial population set up - the population was split 80%/20% for 1000 people to mimick the condition of 800 people attending brunches and 200 attending a wedding as described in the blog post.

2. Infection sampling - random subset of attendees is selected to simulate infections.
   The function used was np.random.choice() at 10% rate as determined by the variable attack rate.
   The sampling frame was all attendees with everyone having an equal chance at being infected.
   The underlying distribution was that each person had an independent 10% probability of being infected.
   Blog procedure: "Suppose that exactly 10% of people at every event are infected, regardless of the type of event. "

3. Primary Contact Tracing - applied to those who were infected to calculate which ones were successfully traced.
   The function used was np.random.rand() with the sample size of 20% of the infected individuals.
   The sampling frame was all infected individuals.
   The underlying distribution involved a Bernoulli trail for each infected person, where each of them had a 20% chance of being traced (using the probability parameter TRACE_SUCCESS.
   Blog procedure: "Suppose that contact-tracing is imperfect, and that due to faulty recall of patients and staff shortages, an infection has only a 20% chance of being traced to a source event."

4. Secondary contact tracing - applied to infected attendees with 2+ primary tacews in the previous step.
   The function used was event_trace_counts[event_trace_counts >= SECONDARY_TRACE_THRESHOLD].index
   The sampling frame was all attendees at each even type, at events wher primary tracing identified 2+ cases.
   The underlying distribution used conditional logic that relied on reaching a threshold to trigger full tracing for all attendees of a given event.
   Blog procedure: "assume there is a “secondary contact tracing” step. If two infections are independently traced to the same source event, a special effort is made to test every person who    attended that event, with the result that 100% of infections associated with that event are identified."



I initially had issues getting to the images in the blog post (wasn't loading despite trying 2 different browsers then eventually loaded after locating the individual post in the larger blog) but my graphs seem to match the general pattern of the blog post, roughly. The smoothness and variability is similar to the blog post but the exact distributions vary slightly which makes sense given that there's a bit of random sampling happenign througout. The 1000 iterations the variability increases which makes for a more blocky visual but this is explained by the fact that we have less data/information to normalize the distribution.

The code has been altered and set with a random seed so that results are reproducible. 

My results are saved as Figure_1 from the 50k run and Figure_2 from the 1k run.

```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
