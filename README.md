<h1 align="center">Act Auth</h1>
<h3 align="center">Specification of Branch January</h3>

# 1. Introduction

The branch January of Act Auth is an application-level framework for both authentication and authorization in distributed, collaborative and centralized systems.

In the Internet, various kinds of information transfer are generated every day, including public information, private information, information that is only exposed to specified objects, and information that is only exposed to specified objects under certain conditions. Among them, non-public information accounts for a large part. In order to avoid these information being acquired by unexpected objects or restrict them, some measures need to be taken between senders and receivers to control.

In transmission, since the information is relatively private and the relative private Internet can be used, while various authentication methods can be used in the public Internet. However, most of the restrictions on information still relied on multi-party agreements or real world laws and regulations, such as restrictions on the use of personal privacy information. So Act Auth is designed to be a protocol that can achieve relatively reliable information transmission and restriction in a low-confidence public Internet environment.

The specification of branch January of Act Auth refers to some classical engineering practice cases, so some of the content may have already been widely used. This branch only gives some definitions and conventions for reference, and introduces some new concepts and terminology conventions, thus it can be used as the transition of the next branch February.

# 2. Notational and Terminology Conventions

## 2.1. Key Words

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [[1]](#7.1.).

## 2.2. Augmented Backus-Naur Form for Objects

All of the syntax and mechanisms specified in this document are described in both prose and the Augmented Backus-Naur Form for Objects (OABNF) based on Augmented Backus-Naur Form (ABNF) that defined by RFC 2234 [[2]](#7.2.) unless stated otherwise. Implementers will need to be familiar with the notation in order to understand this specification. The OABNF includes the following constructs:

```
name = definition
```

The name of a rule is simply the name itself and case sensitive, and separated from its definition by the equal `=` character.

```
implied *LWS
```

The grammar described by this specification is word-based. Except where noted otherwise,  linear whitespace (LWS) can be included between any two adjacent words (token or quoted-string), and between adjacent tokens and delimiters (tspecials), without changing the interpretation of a field. At least one delimiter (tspecials) MUST exist between any two tokens, since they would otherwise be interpreted as a single token.

```
name = multi-line
       definition
```

The indentation consisting of the same number of SPs is used to indicate a rule definition that spans more than one line. Other WSs have no special meaning except for separating text.

```
name = <rule name or rule description>
```

Angle brackets are used in the definition to distinguish each rule name to make it more recognizable. For rules that require a literal description can also be enclosed in angle brackets to indicate that the words of the statement are not separate elements.

```
"literal"
```

Quotation marks surround literal text. Unless stated otherwise, the text is case-insensitive.

```
rule1 | rule2
```

Elements separated by a bar ("|") are alternatives, e.g., "yes | no" will accept yes or no.

```
(rule1 rule2)
```

Elements enclosed in parentheses are treated as a single element. Thus, "(elem (foo | bar) elem)" allows the token sequences "elem foo elem" and "elem bar elem".

```
*#rule
```

The character "\*" and "#" preceding an element indicates repetition and separation. The full form is "\<n\>\*\<m\>#\<c\>element" indicating at least \<n\> and at most \<m\> occurrences of element, each separated by \<c\>. Null elments are allowed in an explicitly declared way, e.g., "\*\#SP( \*ALPHA )" allows the token sequences "ab cd    ef". Default values are 0 and infinity so that "\*(element)" allows any number of elements, including zero; "1\*element" requires at least one; and "1\*2\#SP(element)" allows one or two and separated by SP.

```
[rule1, rule2]
```

Square brackets are used to indicate the list. The list allows any number of elements, including zero. The extended form is "\[\<n\>\*\<m\>element\]" indicating a list including at least \<n\> and at most \<m\> occurrences of element. Thus, "\[foo, bar\]" means a list including foo and bar in order, "\[\*(\*CHAR)\]" means a list including any number of strings with arbitrary length.

```
rule1{rule2, rule3}
```

Curly brackets are used to indicate the mapping, and nested to express objects, e.g., "o{a{r}, b}" means an object including two attributes "a{r}" and "b", and "a" maps to "r".

```
N rule
```

Specific repetition: "\<n\>(element)" is equivalent to "\<n\>\*\<n\>(element)"; that is, exactly \<n\> occurrences of (element). Thus "2DIGIT" is a 2-digit number, and "3ALPHA" is a string of three alphabetic characters.

```
; comment
```

A semi-colon, set off some distance to the right of rule text, starts a comment that continues to the end of line. This is a simple way of including useful notes in parallel with the specifications.

## 2.3. Basic Rules

The following rules are used throughout this specification to describe basic parsing constructs. The US-ASCII coded character set is defined by ANSI X3.4-1986 [[3]](#7.3.).

```
OCTET    =  <any 8-bit sequence of data>
CHAR     =  <any US-ASCII character (octets 0 - 127)>
UPALPHA  =  <any US-ASCII uppercase letter "A".."Z">
LOALPHA  =  <any US-ASCII lowercase letter "a".."z">
ALPHA    =  UPALPHA | LOALPHA
DIGIT    =  <any US-ASCII digit "0".."9">
CTL      =  <any US-ASCII control character
            (octets 0 - 31) and DEL (127)>
CR       =  <US-ASCII CR, carriage return (13)>
LF       =  <US-ASCII LF, linefeed (10)>
SP       =  <US-ASCII SP, space (32)>
HT       =  <US-ASCII HT, horizontal-tab (9)>
<">      =  <US-ASCII double-quote mark (34)>
WS       =  1*SP
LWS      =  [ LF ] 1*( SP | HT )
```

## 2.4. Terminology

Certain security-related terms are to be understood in the sense RFC 4949 [[4]](#7.4.). These terms include, but are not limited to, "access", "access control", "decode", "encode", "hash value", "identity", "sign", "validate", and "verify".

\$ account

A user of application program is a person or an organization who utilizes the application. A user usually has one or more accounts in an application so that the application can identify different users to provide services separately. But it is not always a person or an organization that directly uses an account to access services. It is also possible that another application program is accessing services on behalf of a person or an organization.

\$ authentication

The process of verifying a claim that a system entity or system resource has a certain attribute value.

\$ authorization

a. An approval that is granted to a system entity to access a system resource.

b. A process for granting approval to a system entity to access a system resource.

\$ capability token

A token (usually an unforgeable data object) that gives the bearer or holder the right to access a system resource. Possession of the token is accepted by a system as proof that the holder has been authorized to access the resource indicated by the token.

\$ native application

"A native application has been compiled to run on the hardware/software environment that comprises the targeted architecture. An example of a native Android app would be a mobile application written in Java using the Android toolchain. On the other hand, a Web App that runs inside a browser is not native — it is run in the web browser, which sits on top of the native environment, not the native environment itself." [[5]](#7.5.)

\$ private key

The secret component of a pair of cryptographic keys used for asymmetric cryptography.

\$ signature

A symbol or process adopted or executed by a system entity with present intention to declare that a data object is genuine.

\$ storage repository

Available hardware and software facilities that can provide storage services for an appropriate length of time with both read and write functions usually, such as file systems, stand-alone databases, database clusters, etc.

# 3. Specification Management

Act Auth is continuously revised and updated in order to adapt to the changing practical background. In order to avoid confusion, the content of revision and update needs to undergo a strict and unified audit process, and be distinguished by different identifications. This section elaborates the above content and other specification management rules.

## 3.1. Branch and Version Control

Initially, Act Auth was designed to improve the controllability of resource authorization. But in the process of conceiving, it is found that this goal is incompatible with the existing mainstream technology architecture in the case of insufficient mutual trust between the authorizing party and the authorized party. It was eventually decided that dividing Act Auth into several parallel branches for design, under different technical foundations and application program environments.

In text representation, including this document, the normative naming of "Act Auth" consists of two case-sensitive words "Act" and "Auth", and separated by a single space. Following the grammar and naming rules of each programming language, the naming of libraries can be either pascal case or snake case:

```
DocumentName = "Act Auth"
PascalCaseName = "ActAuth"
SnakeCaseName = "act_auth"
```

The branch name of Act Auth SHOULD be picked from the English word in the twelve month of the Gregorian calendar. The first three letters of the branch name can be taken as abbreviations, or added as suffixes of program libraries names according to pascal case and snake case rules:

```
PascalCaseLibraryName = PascalCaseName PascalCaseBranchName
PascalCaseLibraryName = SnakeCaseName "_" SnakeCaseBranchName
PascalCaseBranchName = "Jan" | "Feb" | "Mar" | "Apr" | "May" | "Jun" | "Jul"
                       | "Aug" | "Sep" | "Oct" | "Nov" | "Dec"
SnakeCaseBranchName = "jan" | "feb" | "mar" | "apr" | "may" | "jun" | "jul"
                      | "aug" | "sep" | "oct" | "nov" | "dec"
```

Version naming follows Semantic Versioning [[6]](#7.6.) rules:

```
Version = ( MAJOR "." MINOR "." PATCH )
MAJOR = 1*DIGIT
MINOR = 1*DIGIT
PATCH = 1*DIGIT
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

- Describe the problems and application scenarios that need to be solved. The solution SHOULD include logical descriptions or (pseudo) code examples, as well as related theories that may be involved.
- Describe potential problems that may exist and their relationships with other features.
- This phase allows the responsible person (or co-responsible persons) adopt and absorb draft proposals from Strawman stage, and allow members of draft proposals to be included in current proposal members.

Draft:

The draft generated at this stage is the first version of the new feature specification.

- In principle, the draft only accepts incremental modification.
- SHOULD contain sufficient logical descriptions and (pseudo) code examples.
- If it affects features that already exist in the specification, compatibility or migration solutions SHOULD be provided.

Candidate:

In the candidate phase, complete feature documents and test cases are provided, and developers try to implement them and give feedback.

Finished:

Ready, this proposal will be included in the next version of the specification.

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

# 4. Centralized Account Model

With the mainstream technology in the times of writing this document, January branch of Act Auth adopts a centralized account model for engineering implementation.

In the centralized account model, account authentication information, such as account identifier, account secret key and so on, is stored in the same storage device or in the same storage cluster. Whether it is stored in centralized database or distributed database, it does not change the nature of centralized account processing. Account authentication information can be stored in multiple copies in case, but only one of them is enabled to avoid data inconsistency caused by read-write conflicts.

## 4.1. Account Registration

Any user who needs to access the services provided by the application program needs to register an account in the application first.

Account registration is not within the scope of this specification, but it usually requires end-users to submit forms through Web pages or native applications, or other application programs to submit forms by HTTP requests.

When an account is being registered, the application program SHALL:

- be clear about the type of this account as described in [Section 4.1.1](#411-account-types),
- generate internal identifiers, external identifiers and private key for this account refer to [Section 4.1.2](#412-internal-account-identifier) and [Section 4.1.3](#413-external-account-identifier),
- store the identifiers and other necessary identity information in authentication center or the location that authentication center has access to.

### 4.1.1. Account Types

Account types are typically delimited according to the services of the application program they access. If the application program only provides one service, there is usually only one type of account; If an application program provides two services, there are usually at most three types of accounts. For example, if a todo-list application program provides both read and write services, it MAY contain up to three types of accounts:

read-account

An account type that can only read todo items from todo-list.

write-account

An account type that can only write todo items to todo-list.

read-write-account

An account type that can either write todo items to or read todo items from todo-list.

In addition, if there are other services, such as different accounts can read different todo items, and / or background management services, there will be more possible account types. However, it is not necessary to enumerate all the account types in the practical application program. It is enough to delimit and design the account types that are actually needed.

The specific delimitation of account types is determined by the designer and developer of the application program. Here are some common scenarios of account type delimitation.

end user

End users of services provided by the application program are the most common type of account. For example, the core service provided by Notepad is a long-term storage repository that can be read and written, and its end users are the people who can access this service or other programs with ability and permission to access (e.g. crawlers). There may be more divisions under this. More types of accounts may be subdivided, depending on actual demands.

application program component

An application program (or a project) with complex implementations may be designed and developed into multiple parts that provide internal and / or external services respectively, e.g., log services, gateway services. These parts can be referred to as components of an application program (or project). Application program components also need to access each other's services, e.g., gateway components may need to access services provided by log components to persist access records. Usually when application program components access each other's services through a non-private network, there also need to be authentication center, and then these application program components can be counted as a special type of account to achieve access control.

third-party

This type of accounts is similar to the aforementioned "application program component", except that the registrants of this type of accounts are not parts of the application program (or project), but the "third-party" that provide services independently. Some of services provided by "third-party" MAY depend on the account system (or other data) of the application program, e.g., "Login with Google".

### 4.1.2. Internal Account Identifier

The internal account identifier is a unique integer (or string, tuple, etc.) representing the registration information provided by the registrant, and used to uniquely identify the information and other resources owned by the registrant within the application program. As the name implies, internal account identifiers SHOULD only be used internally in the authentication center of the application program, unless the internal account identifier is the same as the external account identifier value (strongly NOT RECOMMENDED).

The internal account identifier SHOULD be globally unique in an application program, regardless of the account type, but the generation of identifiers can be related to the account type. If the account information of an application program needs to be stored in different contexts (e.g., divide into different tables according to the account type), it is RECOMMENDED that all account identifiers (rather than the full amount of account information) be duplicated in the same context and stored in one more copy in order to ensure the uniqueness of the identifiers. Take relational database as an example:

```
                                  +------------------------------+
                                  | end-user                     |
                                  |------------------------------|
                                  | internal id | nickname | age |
+---------------------------+     |-------------|----------|-----|
|          account          | +-->| 1           | alice    | 20  |
|---------------------------| |+->| 2           | bob      | 23  |
| internal id | type        | ||  +------------------------------+
|-------------|-------------| ||
| 1           | end-user    |-+|
| 2           | end-user    |--+  +------------------------------+
| 3           | third-party |-+   | third-party                  |
+---------------------------+ |   |------------------------------|
                              |   | internal id | key  | orgname |
                              |   |-------------|------|---------|
                              +-->| 3           | abcd | mofon   |
                                  +------------------------------+
```

Except for the uniqueness of internal account identifiers, this specification does not specify any other restrictions. Since the internal account identifier is only used within the application program, there can be some internal conventions for identifier, e.g., the data type, length, generation method and so on.

### 4.1.3. External Account Identifier

### 4.1.4. Account Metadata

# 7. References

<span id="7.1."></span>7.1. Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

<span id="7.2."></span>7.2. Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", RFC 2234, November 1997.

<span id="7.3."></span>7.3. Coded Character Set--7-Bit American Standard Code for Information Interchange, ANSI X3.4-1986.

<span id="7.4."></span>7.4. Shirey, R., "Internet Security Glossary, Version 2", RFC 4949, August 2007.

<span id="7.5."></span>7.5. chrisdavidmills, klez, hbloomer, Andrew_Pfeiffer, "Native - MDN Web Docs Glossary: Definitions of Web-related terms", Mar 2019.

<span id="7.6."></span>7.6. Tom Preston-Werner, "Semantic Versioning Specification (SemVer)", Jun 2013.

<span id="7.7."></span>7.7. Fielding, R., Ed., "Hypertext Transfer Protocol (HTTP/1.1): Authentication", RFC 7235, June 2014.
