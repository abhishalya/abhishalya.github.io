@def title = "Long challenge problem discussion"
@def published = "15 January 2019"
@def tags = ["cp"]

So finally, long challenge is over which to be honest wasn't very long for me.
I only did 4 problems after which I lost the interest in solving problems.
I prefer the problems that are short and has a good logic or something that is
worth thinking about. So here are my discussions on the problems that I solved
or at-least read. (I'm talking about the Div. 2 here, btw).

# Fancy Quotes - FANCY

This was the first problem of the Div. 2 and as expected it was a pure cakewalk.
The only thing needed here was implementation and honestly I think the code is
way simpler in Python than any other language.

Here is my code which is sweet and short ;)

```py
t = int(input())
while t:
    s = input()
    if 'not' in s.split():
        print('Real Fancy')
    else:
        print('regularly fancy')
    t -= 1
```

I hope the solution is very clear if one has understood the problem. I solved it
in few minutes and moved on to the next problem.

# Lucky Number Game - HP18

This problem was easy, but the wording was WRONG!

I mean it, here is what was wrong with the statement.

>The players alternate turns. In each turn, the current player must remove `a` non-zero number of elements from the sequence

That `a` made me mad because the first time I read this, I read it like we have
to remove a single non-zero element for a turn. Hence I was producing multiple
WAs on submitting. As soon as I fixed the issue, the problem was solved.

There was nothing much other than the implementation for the problem, just a
better statement is always nice to have.

Here is my code if you are interested:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int t; cin >> t;
  while(t--) {
    int n, a, b;
    cin >> n >> a >> b;
    int ca = 0, cb = 0, cc = 0;
    for(int i = 0; i < n; i++) {
      int x; cin >> x;
      if(x % a == 0 && x % b == 0) cc++;
      else if(x % a == 0) cb++;
      else if(x % b == 0) ca++;
    }
    if(cb >= ca) {
      if(cc) cout << "BOB\n";
      else cout << "ALICE\n";
    } else {
      cout << "ALICE\n";
    }
  }
  return 0;
}
```

I try to keep my codes short but readble and clean at the same time. Let me know
if I can improve somewhere.

# Distinct Pairs - DPAIRS

Honestly I didn't know how my solution worked. The problem was simple, basic
implementation and it should produce you an AC.
I think that this problem was nice and much better than the previous two
compared to their difficulty. It took me some time to land upon the required
observation of producing the pairs.

> Hint: If you are in the middle of solving this problem then think the number
of pairs you have to print. You can get the idea of how your solution needs to
be.

This is my code for the problem, I'm pretty sure the author has the same
solution for this problem.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n, m;
  cin >> n >> m;
  int a[n], b[m];
  int mn = INT_MAX, mx = INT_MIN;
  int mni, mxi;
  for(int i = 0; i < n; i++) {
    cin >> a[i];
    if(a[i] < mn) {
      mn = a[i];
      mni = i;
    }
  }
  for(int i = 0; i < m; i++) {
    cin >> b[i];
    if(b[i] > mx) {
      mx = b[i];
      mxi = i;
    }
  }
  for(int i = 0; i < n; i++) {
    if(i != mni) {
      cout << i << " " << mxi << endl;
    }
  }
  for(int i = 0; i < m; i++) {
    cout << mni << " " << i << endl;
  }
  return 0;
}
```

# Chef and Modulo Game - MGAME

Okay, this was the last and probably the most interesting problem I solved
during the long challenge.

The problem was very clear and had some interesting observations. Basic required
thing was the definition of `modulo (%)` operation. Once you split the problem
into basic parts the solution gets clearer and clearer.

I liked this problem and it took me some time to arrive at the solution.

Here is my code for the same:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int t; cin >> t;
  while(t--) {
    int n, p, mx; cin >> n >> p;
    if(n % 2 == 0) mx = n / 2 - 1;
    else mx = n / 2;
    long long ans;
    if(mx == 0) {
      int x = (n == 1) ? 1 : 2;
      ans = 1LL * x * p * p + 1LL * (p - x) * p * x + 1LL * (p - x) * (p - x) * x + 1LL * (p - x) * (p - x) * (p - x);
    } else {
      ans = 1LL * (p - mx) * (p - mx) + 1LL * (p - n) * (p - mx) + 1LL * (p - n) * (p - n);
    }
    cout << ans << endl;
  }
  return 0;
}

```

If you are in doubt of any solution you can mail me, or leave an issue at
[MyCPCodes](https://github.com/abhishalya/MyCPCodes) where I keep my
codes updated.

So this was it, I liked the problem set and it is always fun to upsolve the
long contest problems because of their sheer difficulty.

I will update this post if I solved the remaining problems.

Happy coding!
