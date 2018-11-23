This document describes recommendations for writing quality code for
Status.

## Background

  - Look at these documents for general Go coding standards and
    recommendations:
  - Common mistakes in Go (mostly for Go newcomers):
    <http://devs.cloudimmunity.com/gotchas-and-common-mistakes-in-go-golang/>
  - Effective Go: <https://golang.org/doc/effective_go.html>
  - Code review comments:
    <https://github.com/golang/go/wiki/CodeReviewComments>
  - Each link is an invaluable source of widely accepted practices.

## Recommendations

1.  Be a good git-citizen, keep commit messages to the point, do `git
    pull --rebase` where possible. Read
    <https://www.derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebasefor>
    more details.
2.  When leaving TODO comments, don’t forget to specify your slack
    nickname, for example:`// TODO(tiabc): Fix the underlying
    processing.`This helps other people reach the appropriate person to
    elaborate when needed.
3.  Use `testify` for testing, it’s more verbose than the default
    testing library.
4.  Tests should test what you expect them to test and should NOT test
    unrelated code. Lots of tests in Status needs refactoring.
5.  Tests are preferred to be in `Arrange-Act-Assert` format. It brings
    a unified structure to them and creates eye catchers for better
    navigation, for <example:This> may look like overhead but it brings
    structure, forces you to write different tests for different checks
    and proves fruitful in long-term.

<!-- end list -->

```
  func (s *JailTestSuite) TestParse() {
      // Arrange.
      require := s.Require()
      extraCode := `
      var _status_catalog = {
          foo: 'bar'
      };
      `

      // Act.
      response := s.jail.Parse("newChat", extraCode)

      // Assert.
      expectedResponse := `{"result": {"foo":"bar"}}`
      require.Equal(expectedResponse, response)
  }
```

1.  Before sending a pull request:

<!-- end list -->

  - Review your code yourself and make sure everything is up to your
    code quality bar.
  - Ensure the build is successful.
  - Ensure tests pass.
  - Ensure `go fmt`, `go lint` and `go vet` produce no warnings. If they
    do, fix them or add ignore rules.
  - Make sure you’re sending the pull requests from the same repository
    if you have permissions to create branches.
  - If you’re sending the pull request to ask for advice, explicitly
    state it to the reviewer so that he or she review only what’s
    necessary to answer the question.

From @anna (QA):

> btw, I also like Jan’s style in PR summary content for a bug fix, e.g.
> <https://github.com/status-im/status-react/pull/1509> , it explains
> the cause and the fix done. So, I can get more insights while
> verifying the bug. It might be not applicable in all fixes but I would
> appreciate if there is some notes on how bug is fixed. I will not look
> in the files changed for each PR and will not understand everything in
> the code, so some summary is helpful.