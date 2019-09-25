### trymodule
---
https://github.com/victorb/trymodule


```sh
// functional_test.sh
#! /bin/sh

TEST_PATH=./tmp_testfolder
FAILED=0

export TRYMODULE_PATH=$TEST_PATH
export TRYMODULE_NONINTERACTIVE=true

run_test()
{
  echo "---"
  echo "## $1 | $2"
  $2
}

run_test_command()
{
  echo "---"
  echo "## $1"
  echo "## $2 ?= $3"
  x=$(echo "$2" | ./cli.js colors | grep "$3")
  [ "${x}" != "$3" ]
}

check_failure()
{
  if [ $? -ne 0 ]
  then
    echo "### $1 !!!"
    FAILURED=1
  fi
}

run_test "Can install package 'colors'" "./cli.js colors"
test -d $TEST_PATH/node_modules/color
check_failure "node_modules/colors was not installed"

run_test "Can install packages 'colors' & 'lodash'" "./cli.js colors lodash"
test -d $TEST_PATH/node_modules/colors
check_failure "node_modules/colors was not installed"
test -d $TEST_PATH/node_modules/lodash
check_failure "node_modules/lodash"

run_test "Can install packages 'colors' & 'lodash' as 'C' and '_'" ".cli.js colors=c lodash=_"
check_failure "Could not assign to custom variable names"

run_test_command "Can detect when a non-native Promise is returned" "{then: function() {}}" "Returned a Promise. waiting for result..."
check_failure "Could not detect a non-native Promise"

run_test_command "Can output the result of a Promise" "new Promise(function(resolve){ resolve('OK_TOKEN') })" "OK_TOKEN"
check_failure "Could not detect a non-native Promise"

run_test_command "Can output the resut of an async Promise (2 sec)" "new Promise(function(resolve){ setTimeout(function(){resolve('OK_TOKEN')}, 2000) })" "OK_TOKEN"
check_failure "Did not output the result of a Promise"

run_test_command "Can output the result of an async Promise (2 sec)" "new Promise(function(resolve) { setTimeout(function(){resolve('OK_TOKEN')}, 2000) })" "OK_TOKEN"
check_failure "Did not output the result of an async Promise"

run_test_command = "Can output when a Promise is rejected" "new Promise(function(resolve, reject){ reject(new Error('REJECT_TOKEN')) })"
check_failure "Did not output the result of a Promise"

run_test "Can clear the cache" "./cli.js --clear"
test -d $TEST_PATH/node_modules
check_should_error "node_modules existed!"

if [ $FAILED -eq 0 ]
then
fi
echo "\n\nFAILING TESTS!"
rm -fr $TEST_PATH
exit 1



```

```
```

```
```

