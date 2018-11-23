### Code signing normal commits

TODO. Outside scope of this. PGP standalone works; can also use Yubikey.

### Code signing PRs

Here's the tricky part. See
<https://gist.github.com/oskarth/a6f2e18965372e4e858c16ee5c6d9b95> for
details. Here's a workflow that seems to work for core contributors at
least.

1\. First, submit PR, make sure it gets reviewed and approved.

Now we want to get it into develop without changing the commit SHA.
Sometimes verified commits (usually merge commits) appear as verified,
but this is Github's key.

2\. Pull down the PR's branch, make sure it is squashed to one commit

3\. Fetch latest origin/develop, rebase PR branch on origin/develop

4\. Still in that branch, \`git show --show-signature\` should result in
something like:

\`\`\` commit 90a0058a4391c88a6772bf1d26ea41f51e271f27 (HEAD -\>
develop, origin/feature/spike-offline-inbox, origin/develop,
origin/HEAD, feature/spike-offline-inbox) gpg: Signature made Mon Dec 4
19:14:08 2017 CET gpg: using RSA key
588E96E302D2288D9BABB3D75128AB0637CD85AF gpg: Good signature from "Oskar
Thoren \<ot@oskarthoren.com\>" \[ultimate\] Author: Oskar Thoren
\<ot@oskarthoren.com\> \`\`\`

Note the commit SHA.

5\. \`git push -f\` to that branch to get this SHA as accepted in PR
branch.

6\. Call \`git checkout develop\` and \`git merge
feature/spike-offline-inbox\`. Use fast-forward (default). Note that the
commit SHA should stay the same.

7\. \`git push\`. Despite \`develop\` being a protected branch, the SHA
has been accepted by reviewers so direct push is possible.

8\. Profit\! Your original signed commit is now in develop.