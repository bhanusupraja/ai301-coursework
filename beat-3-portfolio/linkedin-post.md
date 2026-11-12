I just opened a fix for an issue in codex-lb where multi-agent sources were inadvertently stripping out the namespace collaboration tool.

The fix ensures model sources with multi_agent_version correctly retain their collaboration tools so multi-agent workflows function as intended.

PR #2724 is currently open and awaiting maintainer review with all CI checks passing and a Low Risk review from CodeRabbit.

Check out the pull request here: https://github.com/Soju06/codex-lb/pull/2724
