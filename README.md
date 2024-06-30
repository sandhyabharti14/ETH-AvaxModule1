# WhistleblowerPlatform

A decentralized whistleblower platform built on the Ethereum blockchain.

## Description
The WhistleblowerPlatform smart contract provides a decentralized platform for reporting, verifying, and rewarding whistleblower reports. It ensures the integrity and reliability of the reporting process through the use of require(), assert(), and revert() statements. The contract includes functionalities for report submission, verification by an admin, and reward issuance for verified reports.

## Getting Started
## Executing Program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a.sol extension (e.g., whistleblower.sol). Copy and paste the following code into the file:




```bash
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract WhistleblowerPlatform {
    struct Incident {
        address informant;
        string details;
        bool authenticated;
        bool compensated;
    }

    address public chief;
    uint public incidentCount;
    mapping(uint => Incident) public incidents;

    event IncidentReported(uint indexed id, address indexed informant);
    event IncidentAuthenticated(uint indexed id);
    event CompensationGranted(uint indexed id);

    modifier onlyChief() {
        require(msg.sender == chief, "Caller is not the chief");
        _;
    }

    constructor() {
        chief = msg.sender;
    }

    function lodgeComplaint(string memory _details) public {
        require(bytes(_details).length > 0, "Complaint details are required");

        incidents[++incidentCount] = Incident(msg.sender, _details, false, false);
        emit IncidentReported(incidentCount, msg.sender);
    }

    function authenticateComplaint(uint _incidentId) public onlyChief {
        require(_incidentId > 0 && _incidentId <= incidentCount, "Invalid incident ID");

        Incident storage incident = incidents[_incidentId];
        require(!incident.authenticated, "Complaint already authenticated");

        incident.authenticated = true;
        emit IncidentAuthenticated(_incidentId);
    }

    function grantCompensation(uint _incidentId) public onlyChief {
        require(_incidentId > 0 && _incidentId <= incidentCount, "Invalid incident ID");

        Incident storage incident = incidents[_incidentId];
        require(incident.authenticated &&!incident.compensated, "Invalid compensation state");

        incident.compensated = true;
        (bool success, ) = incident.informant.call{value: 0 wei}("");
        require(success, "Compensation transfer failed");

        emit CompensationGranted(_incidentId);
    }

    function validateState() public view {
        assert(incidentCount >= 0);
    }

    function forceRevert(string memory _reason) public pure {
        revert(_reason);
    }

    receive() external payable {
    }
}
```

To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.0" (or another compatible version), and then click on the "Compile whistleblower.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Ensure "WhistleblowerPlatform" is selected in the contract dropdown. Click on the "Deploy" button.

In the "Deployed Contracts" section, expand the WhistleblowerPlatform contract.Under the "authenticateComplaint" function, enter the complaint details.Click the "transact" button next to the authenticateComplaint function.

Under the "grantCompensation" function, enter the compensation details.Click the "transact" button next to the grantCompensation function.Confirm the transaction in MetaMask.







## Authors

- Sandhya Bharti
 bhartisandhya1411@gmail.com



## License

This project is licensed under [MIT](https://choosealicense.com/licenses/mit/) License - see the LICENSE.md file for details
