# AWS SAA-C03 Study Guides — Combined Collection

This repository gathers four independent sets of community study notes for the AWS Certified Solutions Architect – Associate (SAA-C03) exam into one place, so a single search covers all of them. Each set lives in its own top-level folder, copied as plain files from its source repository (no submodules) with its original README and LICENSE (where one exists) kept inside. The content is unchanged from the source commit listed below; credit and copyright belong to the original authors.

- **Best for Last-Minute Revision**: RonitSachdev/aws-saa-c03-guides is widely agreed upon by users on Reddit as the go-to resource for rapid exam-day or final-week skimming. It condenses high-yield service comparisons and core concepts without the fluff of a full textbook.
- **Best for Deep, Domain-Structured Study**: alexalmansa/aws-ssa-notes receives strong praise on r/AWSCertifications for its thoroughness. The notes are organized cleanly by exam domain and include helpful architecture diagrams and flashcards based heavily on Stéphane Maarek's popular course.
- **Best for Printable / Annotatable Study**: yashsinghviwork/aws-saa-c03-notes is noted by recent passers on Reddit as an ideal base set of notes to print out or import into tools like Obsidian so you can add your own margin notes while watching video lectures.
- **Best Community-Maintained Reference**: parthivrmenon/aws_saa is another well-liked repository shared on Reddit that breaks down core AWS services cleanly and actively accepts community pull requests to keep details accurate.


| Folder | Source repo | Source commit | License | Description |
|---|---|---|---|---|
| [`ronitsachdev-aws-saa-c03-guides/`](ronitsachdev-aws-saa-c03-guides/) | [RonitSachdev/aws-saa-c03-guides](https://github.com/RonitSachdev/aws-saa-c03-guides) | [`a71e466`](https://github.com/RonitSachdev/aws-saa-c03-guides/tree/a71e466f02702fe01a6fe414c0b49cb8a673d205) | CC BY 4.0 (LICENSE file; GitHub shows "Other") | An index plus 37 topic guides, each with analogies, exam traps, question patterns and a pocket card |
| [`parthivrmenon-aws_saa/`](parthivrmenon-aws_saa/) | [parthivrmenon/aws_saa](https://github.com/parthivrmenon/aws_saa) | [`60858e2`](https://github.com/parthivrmenon/aws_saa/tree/60858e22fc901530797fb827ab1f4e5c4740908d) | CC BY 4.0 (LICENSE file; GitHub shows "Other") | 35 numbered Markdown chapters following the course order, from IAM to whitepapers |
| [`yashsinghviwork-aws-saa-c03-notes/`](yashsinghviwork-aws-saa-c03-notes/) | [yashsinghviwork/aws-saa-c03-notes](https://github.com/yashsinghviwork/aws-saa-c03-notes) | [`e2d0c84`](https://github.com/yashsinghviwork/aws-saa-c03-notes/tree/e2d0c84422d6c025ad3a190edf0fe814c79d1cd0) | No license | 25 Markdown chapters (Obsidian-style) based on Stephane Maarek's course, plus PDF exports |
| [`alexalmansa-aws-ssa-notes/`](alexalmansa-aws-ssa-notes/) | [alexalmansa/aws-ssa-notes](https://github.com/alexalmansa/aws-ssa-notes) | [`be62671`](https://github.com/alexalmansa/aws-ssa-notes/tree/be62671a460d87a274eb1a6a52a24ca66d1e6318) | No license | ~50 per-service Obsidian notes with a cheat sheet, master index and diagrams |

## Searching across all four

From a local clone:

```sh
# Which files mention S3 Glacier, in any of the four sets?
grep -ril "s3 glacier" .

# Show matching lines with file names, Markdown only
grep -rin --include='*.md' "transit gateway" .

# Search a single set
grep -ril "aurora global" alexalmansa-aws-ssa-notes/
```

On GitHub, scope [code search](https://github.com/search?type=code) to this repo with the `repo:` qualifier, and narrow to one folder with `path:`:

```
repo:jvillalobos22/aws-saa-c03-guides "s3 glacier"
repo:jvillalobos22/aws-saa-c03-guides path:parthivrmenon-aws_saa "kinesis"
```

GitHub code search only indexes the default branch, and does not search inside the PDF files.
