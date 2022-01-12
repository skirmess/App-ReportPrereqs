# NAME

report-prereqs - report prerequisite versions

# VERSION

Version 0.006

# SYNOPSIS

- **report-prereqs** \[**--with-{develop,feature=id}**\] \[URL\]

# DESCRIPTION

The `report-prereqs` utility will examine a `cpanfile`, `META.json`, or
`META.yml` file for prerequisites with [Module::CPANfile](https://metacpan.org/pod/Module%3A%3ACPANfile)
respectively [CPAN::Meta](https://metacpan.org/pod/CPAN%3A%3AMeta). It reports the version of all modules
listed as prerequisites (including 'recommends', 'suggests', etc.). However,
any 'develop' prerequisites are not reported, unless they show up in another
category or the `--with-develop` option is used.

Option `--with-feature` enables optional features provided by a CPAN
distribution. Option may be used more than once.

Versions are reported based on the result of `parse_version` from
[ExtUtils::MakeMaker](https://metacpan.org/pod/ExtUtils%3A%3AMakeMaker), which means prerequisite modules
are not actually loaded. Parse errors are reported as "undef". If a module is
not installed, "missing" is reported instead of a version string.

Additionally, unfulfilled required prerequisites are reported after the list
of all versions.

# OPTIONS

- **--cpanfile \[ FILENAME \]**

    Parse the `filename` with [Module::CPANfile](https://metacpan.org/pod/Module%3A%3ACPANfile) instead of the default behavior.
    If the `filename` is omitted it defaults to `cpanfile` - which is also the
    default.

    Can not be used together with `--meta` or a url.

- **--meta \[ FILENAME \]**

    Parse the `filename` with [CPAN::Meta](https://metacpan.org/pod/CPAN%3A%3AMeta) instead of the default behavior. If
    the `filename` is omitted it defaults to `META.json`.

    Can not be used together with `--cpanfile` or a url.

- **--with-develop**

    Also report develop prerequisites.

- **--with-feature** String: Name

    Specify optional feature to enable. Option may be used more than once.

# EXIT STATUS

The report-prereqs utility exits 0 on success, 1 if an error occurs, and 2 if
invalid command line options were specified.

The state of the prerequisites does have no effect on the exist status.

# EXAMPLES

## Example 1 Using this on Travis CI with `AUTHOR_TESTING`

Add the following lines to `.travis.yml` right before your tests are run,
after all your dependencies are installed.

    - cpanm --verbose --notest --skip-satisfied App::ReportPrereqs
    - report-prereqs --with-develop

## Example 2 Using this on Travis CI without `AUTHOR_TESTING`

Add the following lines to `.travis.yml` right before your tests are run,
after all your dependencies are installed.

    - cpanm --verbose --notest --skip-satisfied App::ReportPrereqs
    - report-prereqs

## Example 3 Using this on AppVeyor with `AUTHOR_TESTING`

Add the following lines to `.appveyor.yml` right before your tests are run,
after all your dependencies are installed.

    - perl -S cpanm --verbose --notest --skip-satisfied App::ReportPrereqs
    - perl -S report-prereqs --with-develop

## Example 4 Using this on AppVeyor without `AUTHOR_TESTING`

Add the following lines to `.appveyor.yml` right before your tests are run,
after all your dependencies are installed.

    - perl -S cpanm --verbose --notest --skip-satisfied App::ReportPrereqs
    - perl -S report-prereqs

## Example 5 Show prerequistes from a cpanfile url

    report-prereqs https://raw.githubusercontent.com/skirmess/App-ReportPrereqs/master/cpanfile

## Example 6 Show prerequisites from a MYMETA.json

    report-prereqs --meta MYMETA.json

# RATIONALE

## Why this instead of [Dist::Zilla::Plugin::Test::ReportPrereqs](https://metacpan.org/pod/Dist%3A%3AZilla%3A%3APlugin%3A%3ATest%3A%3AReportPrereqs)

The [Test::ReportPrereqs](https://metacpan.org/pod/Dist%3A%3AZilla%3A%3APlugin%3A%3ATest%3A%3AReportPrereqs)
[Dist::Zilla](https://metacpan.org/pod/Dist%3A%3AZilla) plugin adds a test to your distribution that
prints all the prerequisites version during testing of your distribution.

The goal of this module is to produce the same output on Travis CI and
AppVeyor without including an additional file to the distribution.

# SEE ALSO

[Dist::Zilla::Plugin::Test::ReportPrereqs](https://metacpan.org/pod/Dist%3A%3AZilla%3A%3APlugin%3A%3ATest%3A%3AReportPrereqs)

# SUPPORT

## Bugs / Feature Requests

Please report any bugs or feature requests through the issue tracker
at [https://github.com/skirmess/App-ReportPrereqs/issues](https://github.com/skirmess/App-ReportPrereqs/issues).
You will be notified automatically of any progress on your issue.

## Source Code

This is open source software. The code repository is available for
public review and contribution under the terms of the license.

[https://github.com/skirmess/App-ReportPrereqs](https://github.com/skirmess/App-ReportPrereqs)

    git clone https://github.com/skirmess/App-ReportPrereqs.git

# AUTHOR

Sven Kirmess <sven.kirmess@kzone.ch>

# CONTRIBUTORS

- Stephan Sachse <ste.sachse@gmail.com>

# COPYRIGHT AND LICENSE

This software is Copyright (c) 2018-2022 by Sven Kirmess.

This is free software, licensed under:

    The (two-clause) FreeBSD License
