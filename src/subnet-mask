#!/usr/bin/env bash

SCRIPT=$(basename $0)
MASK_REGEX='^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}$'

if [ ! -v SIMPLEARGS ] || [ ! -r "$SIMPLEARGS" ]; then
  SIMPLEARGS_DIR="${HOME}/.simpleargs.d"
  SIMPLEARGS="${SIMPLEARGS_DIR}/simpleargs"
  if [ ! -f $SIMPLEARGS ]; then
    [ ! -d $SIMPLEARGS_DIR ] && mkdir $SIMPLEARGS_DIR
    SIMPLEARGS_VERSION='v0.2.0'
    SIMPLEARGS_URL="https://github.com/laurivan/simpleargs/releases/download/${SIMPLEARGS_VERSION}/simpleargs-${SIMPLEARGS_VERSION}"
    which wget &>/dev/null &&
      command=('wget' '-O' "$SIMPLEARGS" "$SIMPLEARGS_URL") ||
      command=('curl' '-o' "$SIMPLEARGS" "$SIMPLEARGS_URL")
    ${command[@]} &>/dev/null || { echo "Couldn't download $SIMPLEARGS_URL" >&2; exit 1; }
  fi
  export SIMPLEARGS
fi

check_mask() {
  local mask="$1"

  if ! grep -E $MASK_REGEX &>/dev/null <<<$mask; then
    echo "Error: $mask does not satisfy regular expression '$MASK_REGEX'" >&2
    exit 1
  fi

  for i in ${mask//./ }; do
    if (( 10#$i < 0 || 10#$i > 255 )); then
      echo "Error: Mask octet $i not in range 0 .. 255!" >&2
      exit 1
    fi
  done
}

check_prefix() {
  local -i prefix="$1"

  if (( prefix < 0 || prefix > 32 )); then
    echo "Prefix length is not in range 0 .. 32!" >&2
    exit 1
  fi
}

mask_to_int() {
  local mask="$1"
  local -i int_mask=0

  for i in ${mask//./ }; do
    (( int_mask <<= 8, int_mask += 10#$i ))
  done

  echo $int_mask
}

print_prefix() {
  local -i mask_int="$1"

  local ones=true
  local bit_str=$(bc <<<"ibase=10;obase=2;$mask_int")
  local count=0

  for (( i=0; i<${#bit_str}; i++ )); do
    local -i bit=${bit_str:$i:1}
    if (( bit == 0 )); then
      ones=false
    fi

    if (( bit == 1 )); then
      if $ones; then
        (( count++ ))
      else
        echo "Error: Subnet mask $MASK ($bit_str) is noncontiguous!" >&2
        exit 1
      fi
    fi
  done

  echo $count
}

print_subnet() {
  local -i mask_int="$1"

  printf '%d.%d.%d.%d\n' \
    $(( mask_int >> 24 & 0xff )) \
    $(( mask_int >> 16 & 0xff )) \
    $(( mask_int >> 8 & 0xff )) \
    $(( mask_int & 0xff ))
}

sa_short_description="Print given subnet mask as network prefix"
sa_long_description=(
"Given a contiguous subnet mask, e.g. \"255.255.255.0\" print the equivalent network prefix \
(\"24\" for the case in mention)." \
"Noncontiguous subnet masks will result in an error."
)

# -------------------------------- simpleargs --------------------------------
. "${SIMPLEARGS}" || { echo "Error loading '${SIMPLEARGS}'" >&2; exit 1; }
sa_parse "$0" \
  -p/--prefix-length \
  @doc="Handle the MASK argument as prefix length instead and print the equivalent subnet mask." \
  @doc="This is the inverse operation of $SCRIPT normal operation, i.e. for each prefix length \
\$l in 0 ... 32 applies: \`$SCRIPT -p \$l | xargs $SCRIPT\` equals \$l." \
  @doc="Equivalently for each contiguous subnet mask \$m in '0.0.0.0' ... '255.255.255.255' \
applies: \`$SCRIPT \$m | xargs $SCRIPT -p\` equals \$m" \
  "<MASK>" \
  @doc="Subnet mask in dot notation."
sa_end_parse $?; sa_process "$@"; sa_end_process $?; eval "set -- ${sa_args}"
# ----------------------------------------------------------------------------

! $p && {
  check_mask $MASK
  let -i mask_int=$(mask_to_int $MASK)
  print_prefix $mask_int
}

$p && {
  check_prefix $MASK
  let -i mask_int=$(( -1 << ( 32 - MASK ) & 0xffffffff ))
  print_subnet $mask_int
}
