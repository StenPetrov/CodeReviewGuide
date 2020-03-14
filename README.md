# Pull Request Guidelines
---
#
# Rationale

Reviewing code is hard. Reviewing code and having your code reviewed is even harder when the author and the reviewer have different concepts and expectations about the process. It is often the case when there are multiple teams involved, each with their own code review culture.

This document aims to increase the efficiency and improve the experience of completing a pull request (PR), as well as to provide a common framework and expectations for all parties involved in code reviews.

Good code reviews have purposes beyond improving code quality [^1]:

- They serve as a learning opportunity for all involved parties.
- They promote building a shared mindset.
- They ensure a repository is coherent in design philosophy.
- They improve the longevity of code
- They can reduce the cost of maintaining the code by improving its readability and testability
- Reviewing code can improve confidence for new team members

# Objectives

1. Improve PR completion performance
  1. by providing guidelines for common situations
  2. by reducing low-impact comments
  3. by outlining a workflow for settling disagreements
2. Provide common understanding of the PR workflow
3. Improve the PR experience for both reviewers and authors

# The Pull Request Lifecycle

_Assuming Git and the simplest possible scenario. Skipping all complexities with adding/merging etc as they are not in scope for this document. Different repos may have different PR lifecycle practices. Depending on the repository, there may be an entirely different way to create PRs_

1. The author creates a branch with their alias and feature name
`git checkout -b dev/alias/feature\&gt;`
Avoid making root branches in the repository, it becomes messy
2. Edit/Add/Rename/Remove content
`git add -A`, for example
1. Commit – package uncommitted changes
`git commit -m 'description for this commit'`
Don't skip the description. It will be used to stage a description for the pull request
4. Push – uploads local changes to the branch to the remote repository
`git push`
5. Create a pull request. Different teams and repositories may have tools to do this, **consult your team**
6. Your pull request now has been created and it needs to be reviewed and approved. Reviewers can see active pull requests in the same web interface or in CodeFlow's Dashboard.
_Depending on the repository settings there may be different conditions that need to be met – resolved comments, number of approvals and a few others._
7. Reviewers can look at the proposed changes, comment, vote +1 on existing comments and set a status:
    7.1. Approved – all good, the reviewer is satisfied with the proposed changes
    7.2. Approved with Suggestions – the reviewer is ok with the proposed changes, but has suggestions for minor improvements. If there are no active comments the author should **consider this status equivalent to**  **Approved**
    7.3. Wait for Author – the reviewer has at least one comment that they feel needs to be resolved before they approve the proposed changes.
**With this status the reviewer commits to respond quickly** to comment replies and further changes. The author should notify the reviewer if the comment has been addressed. It helps to include PR link in the notification.
    7.4. Reject – the reviewer feels this PR should not be completed in its current shape.
**The reviewer must provide justification** , preferably in the PR's global comments section. One good reason, for example, is 5000 modified files with both logic changes and refactoring mixed together, but only for the first iteration (or two?).
This status should be rarely used and may require resolution of disagreements
    7.5. The reviewer can clear their status if they feel an SME should approve the PR or if they can not commit to reviewing the next iterations quickly (within a day, for example when going on a vacation)
8. Authors consider the comments left by reviewers and can
    8.1. Reply to a comment – ask for clarification, respond to a question etc.
**If the comment thread is becoming long (say 4+ comments), the author should reach out to the reviewer and discuss offline**. It is a good idea to summarize the offline conversation as a reply to the thread with [Offline] prefix for other reviewers to see
    8.2. Mark a comment status:
     * Pending – the comment was read and the author is either considering it or working on changes to address it. The author should later change this status to one of the other states.
If a PR is completed with a pending comment, may imply the change will be implemented in a subsequent PR. **This practice is discouraged** , and the author should instead open a work item, reply to the comment with a reference to the work item and mark the comment Resolved.
      * Resolved – the author has implemented the changes as proposed by the reviewer or has replied to the comment if it was a question (and not a suggestion)
      * Won't fix – the author declines changes proposed by the comment.
**The author must reply to the comment with a reason why** they are declining the comment's suggestions. Setting this status for a reactivated comment may need some disagreement resolution

9. Once the author has addressed the comments, they submit another iteration [Goto 2.], until they get the PR approved.
   9.1. Authors can help reviewers find PRs that are ready for review by adding **a label**&quot; **ReadyForReview&quot;** to their PR from the PR's page in visualstudio.com, under the Reviewers panel. Reviewers should remove the label when they are done reviewing the PR.

10. Once the PR is approved and applicable Ownership Enforcer (OE) conditions are met, **the author should complete the PR**.
Overriding OE or completing the PR without approval from someone else may be a compliance issue and should be avoided. It is a good practice to get code reviewed, even if it's in some isolated repository and rarely used.
11. PRs that have not been updated or completed for a long time (2-4 weeks?) **should be abandoned**. Abandoning does not destroy the PRs contents. Anyone working in the repository can abandon old PRs, preferably after contacting the author.
This keeps the Active list easier to read.

---

# Pull Request Concepts
#
## Repository Owners

### Adopt these guidelines for your repository

Add a link to this document in your repository (in README.md or CONTRIBUTING.md) to let authors and reviewers know the guidelines when contributing. Any additional guidelines that are specific to the repository can also be described there.

If there are parts of this document that don't apply to your repository or some that are missing in this document but are needed, add your own rules to CONTRIBUTING.md and indicate which rules don't apply and why. 

### StyleCop\* all the things

If your team feels strongly about how the code should look – have it encoded in StyleCop (\*or any other similar tool). Comments on everything? Whitespaces at the end of lines? Order of fields/properties/etc? Not using DateTime? Naming conventions? No default parameters?

Having requirements that are not obvious to the PR authors is a guaranteed way to slow down PRs, especially authors who are new to the repository. Each PR iteration has the potential to delay a feature by days (weekends, reviewer vacation etc). Having StyleCop rules ensures the author is very aware of the repository rules and spares the need for iterations that don't contribute value but rather address secondary concerns.

- If there's exists a StyleCop rule for something, the author only needs to consider enabled rules and respond to comments with &quot;stylecop&quot;/Won't Fix, preferably with a reference to the specific rule ID so the repository owners can update the set.
- If there aren't existing StyleCop rules, custom one can be implemented if the specific comment shows up often. Here are [some examples](https://msazure.visualstudio.com/One/_git/AD-Platform-Lsm?path=%2Fsrc%2FTools%2FLsmCodeAnalyzers%2FLsmCodeAnalyzers).

### Consider relaxing StyleCop rules on a regular basis (yearly?)

The goal is to make everyone more productive. Occasionally look back and consider the effort spent on complying with each StyleCop rule – did it pay off or not? Do the comments that StyleCop enforced authors to write bring value? Line length? Whitespaces? Naming? Does the new dev tool handle things in a way that makes some rules obsolete?

Keep the rule set up to date, it is subject to change if there is a good reason to.

### Have &quot;master&quot; branch

Most repositories have a &quot;master&quot; branch. Not having one can slow down an author of a PR, not knowing what branch to check out. Optionally have &quot;develop&quot; branch for dev work that gets merged into &quot;master&quot; for deployment; build validation testing can be used before merging to &quot;master&quot;.

### Have Ownership Enforcer

- Enable Ownership Enforcer (OE)
- Set the number of approvals needed to 2+ (OE itself counts as one)
- Reset approval status between iterations
- Require all comments to be resolved before completing the PR
- Disallow authors approving their own PR
- Use owners.txt to identify Subject Matter Experts (SMEs)

#
## Authors' Guidelines

### One task per PR

A PR with a clear specific goal is much easier to review than one with several goals in it. The task itself should be described in the PR's title and description. Reviewers may avoid taking on the burden of reviewing very large and complicated PRs, which can lead to delays for the Author.

### Assume no context

Assume the reviewer, as well as engineers who will work with the code, have _no_ context when they read PR description (for reviewers), comments and names of classes, methods etc.

Consider future contributors to the code that may have no context. Avoid abbreviations, names and comments that require context to be well understood. If \*really\* necessary (for example – enum names serialized in DB), add links in comments that describe the context needed.

Code is read more times than it is written. Having a clear meaning will help both reviewers and future contributors complete their work faster.

### Small, self-contained PRs

The PR should be as small as possible, while still self-contained. For each PR the author should ask themselves **&quot;Can I ship this to prod? Can I ship something smaller to prod that still makes sense?&quot;**

&quot;Self-contained&quot; means

- The change is complete, no loose if/else branches that only contain &quot;TODO&quot; or equivalent exception
- There are tests that cover the change, especially for larger code changes
corollary: the code is testable, properly layered, and abstracted
- Troubleshooting the change is (to acceptable degree) possible via telemetry
corollary: exceptions and log messages include variables

The author should aim to create PRs that they could review themselves reasonably fast (30 min or less?)

### Design code changes in a standalone PR

Design documents tend to get scattered in email, sharepoint sites, in OneNote, Word, PowerPoint documents, wikis...

Consider using a designated folder in the repository and .md documents instead. This way design documentation can be discussed in a PR of its own and the format is compatible with wikis.

At least, when a change has some design document it should be linked in the description of the PR.

### If there is a design document – link it

Design documentation is used rarely as a resource in a PR [^2] and design discussions within the comments of a code-changing PR can lead to delays. If the proposed change is implementing a design that's already been agreed upon, then link the design document in the PR's description so it's readily available to reviewers. Refer questions or comments related to the design back to the design document – if a flaw exists in the design itself then the document should be updated; if the design is good, the reviewer should reconsider their comment.

### Don't design in the comments of a code PR

If the feature or change is big enough to need its own design the author should have such a design agreed upon by some of the Subject Matter Experts (SMEs) that will be reviewing the change(s) before creating a PR.

Design discussions in a PR tend to go on forever, can lead to wasteful heavy refactoring between iterations and cause frustrations in all participants. If there is a disagreement on design, the author should organize a meeting[^3] with SMEs as soon as possible to create and agree upon a design. If the PR differs significantly from the design it may be a good idea to abandon the PR so the existing comments don't spark additional design discussions. If an in-person meeting is not possible, organize an online meeting with voice and (if possible) video.

### Don't do &quot;beautification&quot; PRs

Don't do PRs just to rename a few things or fix spelling in comments (unless publicly visible) or change code layout. **Beautify in a restrained fashion when touching the code for other reasons** , but don't expand the scope of the PR for such changes.
Beautification explicitly means there is no other value added by the PR. Making namings or style consistent across the repository still contributes some value and may deserve a PR of its own.

### … yet Refactoring PRs are OK

Refactoring is more than &quot;beautification&quot; – change to class' structure to make it easier to maintain for example. There should be a goal and it should be stated in the PRs description. **Consult potential reviewers before opening a refactor PR**.

### Don't make the reviewer dig or think

Changes should be made as obvious, self-contained, and easy to read as possible. Reviewers should not be surprised what the code does. Meaningful comments around important parts of code can save a PR iteration when the reviewer requests them. Having a meaningful PR description can save reviewers some time and frustration.

### Match the existing repository patterns

Especially in large repositories with many developers the PR will get reviewed faster if the patterns are familiar to those who work there often and are familiar with the code. Match the code style of the repository as close as possible.

If a pattern is determined to be a bad one (ex: unmaterialized LINQ and Tasks) but it exists in the repository then it should be changed across the repo in a refactoring PR that doesn't change anything else.

### Avoid common antipatterns

Look them up for your specific language, too many to list here. Anti-patterns are likely to draw scrutiny from reviewers and slow down the PR.

### Have unit tests

Especially if the changes are not trivial. Unit tests would also encourage writing testable code, avoiding refactors down the road. Unit tests improve overall development speed as they improve the confidence when code changes in the future and help with refactoring – the same unit tests should pass with refactored prod code.

Not having unit tests or unit tests changes in PRs that are less than trivial is an open invitation for reviewers to ask for them and cause the PR to be delayed.

### Answer clarifying questions in the code

For clarifying questions code authors should consider improving the code so that the next person is will not be puzzled the same way as the reviewer. This may be by adding comments, improving naming, rewriting tricky code.

### Unavailable Reviewer?

If the PR is placed on hold by a reviewer who is not available for more than a day find an alternative reviewer to take on the task. Communicate having a new reviewer to the team; if the team agrees, the Author can remove the unavailable reviewer from the PR.

If all comments are addressed ask the new reviewer to review the PR; if there are open comments – discuss them with the new reviewer.

### Resolved/Won't Fix comments reactivated?

See Disagreement Resolution below

### Each iteration should have fewer substantial changes

Over the course of several iterations each subsequent one should be closer to what the author and reviewers consider good enough to check in. When later iterations introduce major changes, especially after some reviewers have signed off, can have undesired impact on the trust relationships between all involved parties. If there is a need for a major change, one that substantially alters the PR in terms of implemented logic, the author should consider abandoning their current PR, discussing with reviewers offline and creating a new PR.

### Need a PR reviewed even faster?

- Proof-read your own PR as a reviewer to find and fix trivial issues before having to wait for someone else to find them.
- Send a message to reviewer(s) [^4], **include a link to your PR.**
They may not be able to immediately review your PR, sending a message serves as a prompt – it is not an obligation for the reviewer.
- Try using the PR labels as suggested above
- To encourage others to review your PRs - **review their PRs**


#
## Reviewers' Guidelines

### **Be respectful and polite**

Review the PR as if it comes from the CEO.

- Don't ask questions you know the answer of, make suggestions instead.
- Avoid using &quot;you&quot;, &quot;your&quot; etc. Focus on the code instead.
- Discuss the code, not the author.
- Provide support for your comment in the form of facts, experiences, links to documentation etc.
- Consider your statements – reviewers can be wrong too
- Provide _actionable_ feedback. The author should know what your comment is asking them to do within the context of the PR.

### Spend extra effort on the first iteration

PRs with too many iterations are not a good sign and are stressful for their authors. Consider the nit comments in late iterations – they likely won't improve the change much but can cause delays and frustration. If a nit comment is to be made in a late iteration and the comment is not on

Also look for the big and important things in the first iteration – architecture, security, overall direction, unit tests and testability... These are not easy to change, and the author may have to do redundant work just because a Big Thing™ was missed in the first iteration or two. If such a thing were missed, consider if it could be split into a separate PR.

### Review iterations as quickly as reasonably possible

Authors will respond faster and offer less push back if their changes are reviewed quickly. Important comments or ones that ask for difficult changes in next iteration are less likely to get push back or lead to disagreements if the author's past experiences show the reviewer is committed to the process.

Not reviewing PR iterations quickly, especially when the reviewer has made their status &quot;Waiting for Author&quot;, can seriously impact productivity and cause frustration. Reviewers should anticipate having to **review another iteration of a PR within a day**.

Reviewers should avoid placing a PR on hold when they know they won't be available to review subsequent iterations for a long time – for example when leaving on a vacation or having extremely busy schedule. In those cases the reviewer should delegate the PR review to another team member.

### Mark high-priority comments with &quot;!!&quot;

These are the comments that the reviewer has strong opinion on and would have the reviewer hold the PR until the comment is resolved or a satisfactory response is provided. Such comments should be used sparingly.

### Mark low-priority comments with &quot;nit:&quot;

These are comments that don't contribute much value and may be ignored by the author and may not even be worth the &quot;Approved with Suggestions&quot; reviewer status.

### Provide rationale when reactivating comments

If a comment was marked Resolved/Won't Fix by the author and the reviewer is not satisfied with the resolution or Won't Fix, then the reviewer can reactivate the comment. In this case the **reviewer must provide an explanation** in the comment thread. A good explanation should show the author that the reviewer understood their point of view and give additional information.

If the reviewer feels a comment needs to be reactivated second time they should first reach out to the author and discuss offline.

### Encourage good patterns

Code reviews are a learning opportunity for both author and reviewer. Leave a compliment comment when you find a good pattern, extra effort etc. A simple &quot;👍&quot; would do. This can be especially useful when reviewing code authored by new members of the team, so they feel included and appreciated and adapt to the repository faster.

Thoughtfully used positive/compliment comments can also improve overall performance in the repo by reducing push back in the future.

### There's no &quot;perfect&quot; code

There's &quot;better&quot; code. If the changes proposed in a PR would definitely improve the overall state of the repository, the reviewer should feel encouraged to approve it, even if it's not perfect. This is especially true for long-running PRs.

### Review as a routine

Informally designate some time (30min?) every day and look for PRs that need review. Doing this as a team ensures there's always someone to review PRs, including ones the reviewer will author.

## Disagreement Resolution

Consider this hierarchy:

1. Security, Privacy
2. Stable Runtime
3. Feature Set, Code Runtime Performance
4. COGS (Cost)
5. Supportability (troubleshoot via telemetry)
6. Code simplicity/readability
7. Code Style Rules/opinions
8. Personal Preferences

If there is an obvious way to assign a category from the above list to the disagreeing sides, the higher priority should be the way to go. For example, the author should not compromise runtime performance to fit the code to a styling opinion but should make changes if the suggested changes would make the code more stable or secure at runtime.

Disagreements should not be a reason for a PR to stall.

- Talk in person or chat. Most disagreements should be resolved here.
- Involve another team member to the review - in person or chat or call; avoid just sending an email. Strive to resolve the disagreement on this step.
- Involve technical lead(s) or other engineer(s) in the discussion.
- Involve a manager; maybe it wasn't resolved because it's a business decision and not an engineering one?
- Things should not have gotten here. Go grab a real or proverbial beer, go for some outside activity.

[^1]: Reasons for code reviewing
[https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/MS-Code-Review-Tech-Report-MSR-TR-2016-27.pdf](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/MS-Code-Review-Tech-Report-MSR-TR-2016-27.pdf), Table 6

[^2]: Additional resources used during code review
[https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/MS-Code-Review-Tech-Report-MSR-TR-2016-27.pdf](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/MS-Code-Review-Tech-Report-MSR-TR-2016-27.pdf), Table 22

[^3]: MSR found PR comments and in-person meetings are preferred communication channels.
[https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/MS-Code-Review-Tech-Report-MSR-TR-2016-27.pdf](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/MS-Code-Review-Tech-Report-MSR-TR-2016-27.pdf), table 11.

[^4]: Teams' preferred communication channels may differ. By default IM a reviewer directly, but some teams may have a dedicated IM channel for PRs and the author should use the channel agreed upon by the team.
