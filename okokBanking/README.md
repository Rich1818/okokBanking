Hi, thank you for buying my script, I'm very grateful!

If you need help contact me on discord: okok#3488
Discord server: https://discord.gg/FauTgGRUku

2. To add salary/paycheck transactions navigate to 'es_extended/server' and open 'paycheck.lua'.

2.1. Add the following code (just like in the example below).

TriggerEvent('okokBanking:AddTransferTransactionFromSocietyToP', salary, "salary", "Salary", xPlayer.identifier, xPlayer.getName())

Example:

if salary > 0 then
	if job == 'unemployed' then -- unemployed
		xPlayer.addAccountMoney('bank', salary)
		TriggerEvent('okokBanking:AddTransferTransactionFromSocietyToP', salary, "salary", "Salary", xPlayer.identifier, xPlayer.getName())
		TriggerClientEvent('esx:showAdvancedNotification', xPlayer.source, _U('bank'), _U('received_paycheck'), _U('received_help', salary), 'CHAR_BANK_MAZE', 9)
	elseif Config.EnableSocietyPayouts then -- possibly a society
		TriggerEvent('esx_society:getSociety', xPlayer.job.name, function (society)
			if society ~= nil then -- verified society
				TriggerEvent('esx_addonaccount:getSharedAccount', society.account, function (account)
					if account.money >= salary then -- does the society money to pay its employees?
						xPlayer.addAccountMoney('bank', salary)
						account.removeMoney(salary)
						TriggerEvent('okokBanking:AddTransferTransactionFromSocietyToP', salary, "salary", "Salary", xPlayer.identifier, xPlayer.getName())
						TriggerClientEvent('esx:showAdvancedNotification', xPlayer.source, _U('bank'), _U('received_paycheck'), _U('received_salary', salary), 'CHAR_BANK_MAZE', 9)
					else
						TriggerClientEvent('esx:showAdvancedNotification', xPlayer.source, _U('bank'), '', _U('company_nomoney'), 'CHAR_BANK_MAZE', 1)
					end
				end)
			else -- not a society
				xPlayer.addAccountMoney('bank', salary)
				TriggerEvent('okokBanking:AddTransferTransactionFromSocietyToP', salary, "salary", "Salary", xPlayer.identifier, xPlayer.getName())
				TriggerClientEvent('esx:showAdvancedNotification', xPlayer.source, _U('bank'), _U('received_paycheck'), _U('received_salary', salary), 'CHAR_BANK_MAZE', 9)
			end
		end)
	else -- generic job
		xPlayer.addAccountMoney('bank', salary)
		TriggerEvent('okokBanking:AddTransferTransactionFromSocietyToP', salary, "salary", "Salary", xPlayer.identifier, xPlayer.getName())
		TriggerClientEvent('esx:showAdvancedNotification', xPlayer.source, _U('bank'), _U('received_paycheck'), _U('received_salary', salary), 'CHAR_BANK_MAZE', 9)
	end
end

3. To replace the "okokBanking" logo, simply replace it with yours on the 'web' folder.