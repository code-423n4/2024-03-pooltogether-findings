[L-01] PrizeVault.sol#depositWithPermit() - enforcing ``msg.sender`` to be the same as the owner variable defeats the purpose of the ``permit`` usage. Permits are a workaround that provides the ability to have a 3rd party pay the gas instead of the owner. Enforcing the owner and sender to match reduces it down to a regular ``approve``, killing the permit's purpose:
``
if (_owner != msg.sender) {
            revert PermitCallerNotOwner(msg.sender, _owner);
        }
``
Recommendation: opt in to just using a regular ``approve`` and not worry about the receiver argument. Similiar to: https://solodit.xyz/issues/m-25-depositwithpermit-and-mintwithpermit-are-allowed-to-be-called-by-the-permit-creator-only-code4rena-pooltogether-pooltogether-git