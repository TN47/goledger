Command line accounting
=======================

[![Join the chat at https://gitter.im/tn47/goledger](https://badges.gitter.im/tn47/goledger.svg)](https://gitter.im/tn47/goledger?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://travis-ci.org/tn47/goledger.svg?branch=master)](https://travis-ci.org/tn47/goledger)
[![GoDoc](https://godoc.org/github.com/tn47/goledger?status.png)](https://godoc.org/github.com/tn47/goledger)

Inspired by [ledger-cli](http://ledger-cli), goledger is a re-write of command
line ledger in golang, with the stated goals.

* Keeping it command line friendly, whether or not GUI / web interface
are available.
* Defaults to Locale en_IN, until the tool becomes smart enough
to handle locale specific details automatically.
* Targeted for personal, small and medium enterprises.
* Keep to the spirit of ledger-cli as much as possible.

Basic Concepts
==============

* ``Credit the giver debit the receiver``.
* ``Commodity``, based accounting, where currency is also a commodity.
* ``Account``, holding one or more commodities.
* ``Posting``, commodities can be posted to an account. Posting can be
debit posting or credit posting, based on whether the account is the
source account (credit the giver) or target account (debit the receiver)
* ``Transaction``, involving 2 or more postings, where the sum of credit
postings should balance out the sum of debit postings.

Once comfortable with above concepts, your can start book-keeping with ledger.

Transactions and Directives
===========================

Book-keeping starts with the journal file, typically ending with ``.ldg``. A
ledger file can contain directives (sometimes called as declarations) or
transaction entries.

There can be any number of directives and any number of transaction entries.

Transactions have the following format:

```text
2011/03/15   Whole Food Market
    Expenses:Groceries   75.00
    Assets:Checking      -75
```

**line1**: Date and Payee  
**line2**: Accountname, Amount  
**line3**: Accountname, Amount  

* Each transaction is associated with a date, and it is always advised to
enter transaction with older date before and newer date after.
* Payee gives a name to the transaction, other than that no special
meaning is attached to it.
* Posting with positive amount are treated as debit transaction, and the
posting's account is called target account.
* Posting with negative amount are treated as credit transaction, and the
posting's account is called source account.

Standards, conventions and views
--------------------------------

While the basic concept of accounting is not more that what is stated
above, there are endless variations stipulated by accounting standards
and conventions. Sometimes we may need different view of accounts for the
same set of transactions. For example, let us take accrual accounting:

**Accrual accounting**

Cash (or commodities) are physical and to get a __realistic__ picture of
our accounts we book transactions that has already taken place
physically. Now, we will look at three different scenarios demanding
three different flavours of __realism__.

**Scenario1: Income Tax filing**

At least in India income tax department expects us to accrue all interest
that are receivable on Bank deposits, as income, and pay tax. Although we
might not have received the interest until the deposit matures. How to
book this in goledger:

```text
2016/Jun/21  FD savings ; Deposit for 1 year
    Asset:FD                        10000
    Asset:CurrentAccount

2017/Mar/31  Accrued interest on Bank FD
    Income:Interest:Receivable       800
    Income:FD-Interest

2017/Jun/22  FD Matures
    Asset:CurrentAccount            11000
    Income:Interest:Receivable       -800
    Income:FD-Interest               -200
    Asset:FD                       -10000
```

**Scenario2: Distress sale of company**

When a company is under distress and a valuation is to be done to attract
potential buyers, it is normally expected that buyer would ask us to
discount Receivables but include Payables.

**Scenario3: Brokerage houses**

If book keeping is done for a brokerage house, which buy commodity and sell
them later, most often the actual payment will be deferred until actual sale
has happened. For them, accrual accounting might have to be totally skipped
while generating a report on operating-cost.

As evident in above case, such variations are endless and hence, goledger
doesn't try to be smart about them. Just ``Credit the giver and debit the
receiver``. If a particular feature or a nicety is required in booking
a transaction or in generating a report, please file an issue and let us
see whether or how to take it forward.

Getting Started
===============

There are plans to package ledger for different platforms, like windows,
ubuntu, mac, raspberry-pi, debian etc.. Until then, goledger can be
obtained via golang-tools. If you are a mac user or linux user,
[install Golang](https://golang.org/doc/install), and:

```bash
$ go get github.com/tn47/goledger
$ cd tn47/goledger
$ make test # sanity test.
$ make install
```

It is surprisingly easy!!

Gotchas
=======

* Account name should start with at least 2 character
* Hard spaces (2 or more consecutive spaces or a single tab-space) have
special meaning in ledger format. So avoid them in account names.

Ledger-likes
============

* [ledger rewrite in haskell](https://github.com/simonmichael/hledger)
* [ledger rewrite on JVM](https://github.com/sn127/tackler)
* [ledger rewrite in scala](https://github.com/hrj/abandon)
* [ledger-cli](https://github.com/ledger), it is worth spending time reading
upon the ledger specification.

How to contribute
=================

* Pick an issue, or create an new issue. Provide adequate documentation for
the issue.
* Assign the issue or get it assigned.
* Work on the code, once finished, raise a pull request.
* Goledger is written in [golang](https://golang.org/), hence expected to follow the
global guidelines for writing go programs.
* If the changeset is more than few lines, please generate a
[report card](https://goreportcard.com/report/github.com/tn47/goledger).

Happy Accounting !!
