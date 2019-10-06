<h1 align="center">Act Auth</h1>
<h3 align="center">Specification of Branch January</h3>

# 1. Introduction

The Act Auth is an application-level protocol for both authentication and authorization in distributed, collaborative and decentralization systems.

In the Internet, various kinds of information transfer are generated every day, including public information, private information, information that is only exposed to specified objects, and information that is only exposed to specified objects under certain conditions. Among them, non-public information accounts for a large part. In order to avoid these information being acquired by unexpected objects or restrict them, some measures need to be taken between senders and receivers to control.

In transmission, since the information is relatively private and the relative private Internet can be used, while various authentication methods can be used in the public Internet. However, most of the restrictions on information still relied on multi-party agreements or real world laws and regulations, such as restrictions on the use of personal privacy information. So Act Auth is designed to be a protocol that can achieve relatively reliable information transmission and restriction in a low-confidence public Internet environment.

# 2.

# 3. Specification Management

Act Auth is continuously revised and updated in order to adapt to the changing practical background. In order to avoid confusion, the content of revision and update needs to undergo a strict and unified audit process, and be distinguished by different identifications. This section elaborates the above content and other specification management rules.

## 3.1. Branch and Version Control

Initially, Act Auth was designed to improve the controllability of resource authorization. But in the process of conceiving, it is found that this goal is incompatible with the existing mainstream technology architecture in the case of insufficient mutual trust between the authorizing party and the authorized party. It was eventually decided that dividing Act Auth into several parallel branches for design, under different technical foundations and application environments.

In text representation, including this document, the normative naming of "Act Auth" consists of two case-sensitive words "Act" and "Auth", and separated by a single space. Following the grammar and naming rules of each programming language, the naming of libraries can be either pascal case or snake case:

```bash
DocumentName = "Act Auth"
PascalCaseName = "ActAuth"
SnakeCaseName = "act_auth"
```

The branch name of Act Auth is picked from the English word in the twelve month of the Gregorian calendar. The first three letters of the branch name can be taken as abbreviations, or added as suffixes of program libraries names according to pascal case and snake case rules:

```bash
PascalCaseLibraryName = PascalCaseName PascalCaseBranchName
PascalCaseLibraryName = SnakeCaseName "_" SnakeCaseBranchName
PascalCaseBranchName = "Jan" | "Feb" | "Mar" | "Apr" | "May" | "Jun" | "Jul"
                       | "Aug" | "Sep" | "Oct" | "Nov" | "Dec"
SnakeCaseBranchName = "jan" | "feb" | "mar" | "apr" | "may" | "jun" | "jul"
                      | "aug" | "sep" | "oct" | "nov" | "dec"
```

Version naming follows Semantic Versioning [[4]](#x.4.) rules:

```bash
Version = ( MAJOR "." MINOR "." PATCH )
MAJOR = NonnegativeInteger
MINOR = NonnegativeInteger
PATCH = NonnegativeInteger
NonnegativeInteger = < integer greater than or equal to 0 >
```

In the active state, the replacement of the main version number means that there are destructive updates and new features; a downward compatible version with new features is released regularly with the replacement of the minor version number; defect fixes or chores are merged into the new revision version from time to time, depending on the situation. The replacement frequency of the minor version number and the patch version number is determined by the specific implementer.

Version conditions can be indicated by "^", "~", ">=", "<=", ">", "<". e.g., "ActAuthJan@1.2.3" refers to the specific version of branch January with the main version number 1, the minor version number 2 and the revised version number 3; "act_auth_jan@>1.0.0" allows any versions of branch February with the major version number greater than or equal to 1, and the minor version number and the revision version number greater than or equal to 0 except version 1.0.0; "ActAuthJan@~2.1.0" is equivalent to the intersection of "ActAuthJan@>=2.1.0" and "ActAuthJan@<2.2.0"; "ActAuthJan@^2.3.0" is equivalent to the intersection of "ActAuthJan@>=2.3.0" and "ActAuthJan@<3.0.0".

This document is the specification of Act Auth January branch. In the following sections, the word "Act Auth" refers to the "January" branch except for specific instructions.

## 3.2. Feature Addition / Modification Process

Under normal circumstances, each new feature or feature modification of Act Auth needs to go through the following five stages to be adopted in the specification:

Strawman:

Anyone at this stage can submit a draft proposal (different from "draft").

- The current submission channel is Github Issue, which will be migrated one after another after the official website goes online.
- The official website provides a reporting mechanism, and the reported users will be restricted when submitting draft proposals.

Proposal:

A formal proposal is generated at this stage.

- Determine proposal head (or combine head), head (Or co-owner) Must be from the Act Auth Team.
- Describe the problems and application scenarios that need to be solved. The solution must include logical descriptions or (pseudo) code examples, as well as related theories that may be involved.
- Describe potential problems that may exist and their relationships with other features.
- This phase allows the responsible person (Or co-responsible persons) adopt and absorb draft proposals from stage 0, and allow members of draft proposals to be included in current proposal members.

Draft:

The draft generated at this stage is the first version of the new feature specification.

- In principle, the draft only accepts incremental modification.
- Must contain sufficient logical descriptions and (pseudo) code examples.
- If it affects features that already exist in the standard, compatibility or migration solutions must be provided.

Candidate:

In the candidate phase, complete feature standard documents and test cases are provided, and developers try to implement them and give feedback.

Finished:

Ready, this proposal will be included in the next version of the standard.

## 3.3. Audit Rules

### 3.3.1. Feature Addition / Modification

From Strawman to Draft: Both rounds of audit require the consent of at least one Act Auth Team member who did not participate in this feature addition or modification.

From Draft to Candidate: Require the consent of at least two Act Auth Team members who did not participate in this feature addition or modification, without any member's objection.

From Candidate to Finished: Require the consent of at least three Act Auth Team members who did not participate in this feature addition or modification.

### 3.3.2. Revocation of Feature Addition / Modification

Strawman and Proposal: The owner of this feature addition or modification can revoke it without anyone's approval.

Draft and Candidate: The owner of this feature addition or modification submits the revocation application, with the approval of at least three Act Auth Team members who did not participate in.

Finished: Revocation is not allowed. Can submit a new feature modification.

### 3.3.3. Change of Feature Addition / Modification Members

The owner of this feature addition or modification can add or remove members without anyone's approval. Members are free to leave. If the owner leaves, the remaining members can discuss electing a new owner. When all members left, the feature addition or modification will be revoked automatically.

### 3.3.4. Act Auth Team

Member:

- The person who has at least one adopted feature addition or at least two adopted feature modifications can apply to become a Member.
- Collaborator with significant contribution can apply to become a Member.

Collaborator:

- The person who has participated at least one adopted feature addition or at least two adopted feature modifications can apply to become a Collaborator.

Removal:

- Twice major mistakes.
- On the restricted list lasts for more than a month.

### 3.3.5. Other

- If the Act Auth Team members are insufficient, the minimum number of audit personnel can be changed as appropriate.
- Persons on the restricted list are not allowed to participate in the feature adoption or modification except have already participated.

# x. References

<span id="x.1."></span>x.1. Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

<span id="x.2."></span>x.2. Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", RFC 2234, November 1997.

<span id="x.3."></span>x.3. Coded Character Set--7-Bit American Standard Code for Information Interchange, ANSI X3.4-1986.

<span id="x.4."></span>x.4. Tom Preston-Werner, "Semantic Versioning Specification (SemVer)", Jun 2013.
