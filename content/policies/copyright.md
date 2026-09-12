---
title: "Copyright Policy"
weight: 1
summary: "Licensing, attribution, and the Developer Certificate of Origin."
---

Gravity Linux is an open source project, and all contributors must abide by
standard open source norms and practices when contributing to an open source
project.

## Licensing

Code developed directly for Gravity Linux should, by default, be licensed under
the GNU GPL v2, unless there are specific reasons why either a different
copyleft license or a permissive license should be used. Internally, we use
[SPDX license
identifiers](https://spdx.github.io/spdx-spec/v2.3/using-SPDX-short-identifiers-in-source-files/)
to record the license of a file.

Code meant to be contributed upstream to another open source project should be
licensed according to upstream convention.

## Attribution

The Git history is a durable record of copyright. As such, the author field
must be the person primarily responsible for writing said code. If the code in
question was authored by multiple people, add `Co-developed-by: Example
Coauthor <foo@example.com>` lines to your commit message accordingly. Each
`Co-developed-by:` line must be immediately followed by a `Signed-off-by:` line
from that co-author.

If an LLM was involved in producing a contribution, our [LLM Use and
Clean-Room Policy](/policies/llm/) sets out the disclosure required in
addition to the attribution rules above.

Non-Git releases of the software will be arranged to have an automatically
generated authorship file containing a list of all Git contributors.

All contributors are required to accept the Developer Certificate of
Origin 1.1:

> ## Developer Certificate of Origin 1.1
>
> By making a contribution to this project, I certify that:
>
> (a) The contribution was created in whole or in part by me and I have the
>     right to submit it under the open source license indicated in the file; or
>
> (b) The contribution is based upon previous work that, to the best of my
>     knowledge, is covered under an appropriate open source license and I have
>     the right under that license to submit that work with modifications,
>     whether created in whole or in part by me, under the same open source
>     license (unless I am permitted to submit under a different license), as
>     indicated in the file; or
>
> (c) The contribution was provided directly to me by some other person who
>     certified (a), (b) or (c) and I have not modified it.
>
> (d) I understand and agree that this project and the contribution are public
>     and that a record of the contribution (including all personal information
>     I submit with it, including my sign-off) is maintained indefinitely and
>     may be redistributed consistent with this project or the open source
>     license(s) involved.

To certify this, add a line to the end of your Git commit message as follows:

```
Signed-off-by: Random J Developer <random@developer.example.org>
```

This can be automated by simply using `git commit -s`.

Policy adapted from Asahi Linux's Copyright policy.
