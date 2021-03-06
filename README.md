# klotan

## Description

Klotan is a Python library used to match a structure (tuples, lists, dicts) with a template.
A template is made of a set of criteria.

## Installation

You just have to clone this repository and install it using the command
```bash
pip install .
```

Or you can also install it from PyPi
```bash
pip install klotan
```

You can test the installation by running the following command :

```bash
python -c "import klotan; print(klotan.match.match([klotan.criteria.equals(22)], [33]).is_valid())"
```

which should print `False`

## Usage

Klotan can be used to validate some form results for example

```python
person_template = {
    # The gender can be either M, F or N/A
    "gender": criteria.is_in("M", "F", "N/A"),
    # Expects a name starting with a capital letter, followed by 1 to 15 letters
    "name": criteria.regex("[A-Z][a-z]{1,15}"),
    # Jeanne Calment still holds the world record
    "age": criteria.between(0, 122),
    # Matches all emails
    "email": criteria.is_email(),
    # Optional field, matches if value is an URL
    match.optional("personal_website"): criteria.is_url(),
    # Matches valid IBANs
    "iban": criteria.is_valid_iban(),
    # Matches valid credit card numbers
    "credit_card": criteria.is_valid_credit_card_number(),
    # We only accept people without debts !
    "amount_of_money": criteria.is_positive(),
    # This criteria will accept anything
    "bonus": criteria.accept(),
}

# Those are of course, fake informations
person = {
    "gender": "M",
    "name": "Bob",
    "age": 18,
    "email": "bob.dupont@gmail.com",
    "personal_website": "https://bob.dupont.fr/",
    "iban": "FR68 1009 6000 7018 7224 1164 C75",
    "credit_card": "4929609419559701",
    "bonus": lambda: 0,
}

print(match.match(person_template, person).to_string())
```

The code above prints the following output :
```python
{
    "gender": Criteria<is_in>("M", "F", "N/A") << ("M") => True,
    "name": Criteria<regex>("[A-Z][a-z]{1,15}") << ("Bob") => True,
    "age": Criteria<between>(0, 122) << (18) => True,
    "email": Criteria<is_email> << ("bob.dupont@gmail.com") => True,
    "personal_website": Criteria<is_url> << ("https://bob.dupont.fr/") => True,
    "iban": Criteria<is_valid_iban> << ("FR68 1009 6000 7018 7224 1164 C75") => True,
    "credit_card": Criteria<is_valid_credit_card_number> << ("4929609419559701") => True,
    "amount_of_money": Criteria<expected>(Criteria<is_positive>, False) << (None) => False,
    "bonus": Criteria<accept> << (<function <lambda> at 0x000001C4767330D0>) => True
}
```

## Licensing

Klotan uses the MIT license, you can find more details about it [here](LICENSE)

## Contributing

You can find more details about contributions [here](CONTRIBUTING.md)