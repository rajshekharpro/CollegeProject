# BookMyTable — Restaurant Table Booking System

An online restaurant reservation system built with ASP.NET MVC 3. Diners browse a
restaurant's menu and offers, check live table availability, and book a table without
phoning in; staff manage tables, menus, employees, and incoming bookings from an admin
area.

## The problem

Small restaurants still take reservations over the phone. That means double-bookings when
two staff answer at once, no searchable record of who is coming and when, and no easy way
to push seasonal offers to people who are already deciding where to eat. BookMyTable moves
the whole flow online and keeps table state in one place.

## Features

- **Table booking** — browse availability by date, reserve a table, confirm, and view or
  edit an existing booking
- **Menu management** — menu items with per-day serving schedules
- **Offers & coupons** — seasonal offers and coupon codes surfaced during booking
- **Billing** — booking bills generated per reservation
- **Accounts** — register, log in, change password, plus Facebook OAuth sign-in
- **Staff/admin area** — manage tables, menu items, offers, and employee records
- **Server-side + unobtrusive client validation** on all forms

## Tech stack

| Layer | Technology |
|---|---|
| Framework | ASP.NET MVC 3 (.NET Framework 4.0) |
| Language | C# |
| Views | Razor / WebForms views, shared `_Layout` |
| Database | Microsoft SQL Server Express 2008 |
| Data access | Hand-written repository pattern over ADO.NET (`IDataRepository` + per-entity repositories) — no ORM |
| Auth | Custom SQL membership, role, and profile providers; Facebook OAuth |
| Client-side | jQuery 1.4.4, jQuery UI, jQuery Validate (unobtrusive), Nivo Slider |

## Architecture

```
RestaurantBookingSystem/
├── Controllers/          Account, Bookings, Customers, Employee, Home,
│                         Images, Menu, OAuth, Offers, Tables
├── Models/               View models (booking, menu, offers, employee, navigation)
├── Views/                Razor views per controller + Shared/_Layout
├── Infrastructure/
│   ├── DataEntities/     RestaurantBooking, RestaurantTable, RestaurantMenuItem,
│   │                     RestaurantUser, BookingBill, Coupon, SeasonalOffer,
│   │                     OfferBase, FacebookUserDetail
│   ├── Repositories/     IDataRepository + one repository per entity
│   ├── Providers/        Custom SQL membership / role / profile provider wrappers,
│   │                     cookie-based TempData provider
│   ├── RestaurantUserIdentity.cs
│   ├── ActionResultNotification.cs
│   └── PaginatedList.cs
├── Content/              Site.css, jQuery UI themes, Nivo slider assets
└── Scripts/              jQuery + validation + AJAX libraries, app scripts
```

Controllers stay thin and talk to repositories through `IDataRepository`, so the data layer
can be swapped or mocked without touching the MVC surface. Paging is handled generically by
`PaginatedList<T>`, and user-facing status messages flow through
`ActionResultNotification` rather than ad-hoc `ViewBag` strings.

## Running locally

**Requirements:** Visual Studio (2010 or later), .NET Framework 4.0, IIS Express, and a
SQL Server instance (developed against SQL Server Express 2008).

1. Clone the repo and open `RestaurantBookingSystem/RestaurantBookingSystem.csproj`.
2. Create a database named `RestaurantBookingDB` on your SQL Server instance.
3. Point the `RestaurantDB` connection string in `Web.config` at your instance:

   ```xml
   <connectionStrings>
     <clear />
     <add name="RestaurantDB"
          connectionString="Data Source=.\SQLEXPRESS;Initial Catalog=RestaurantBookingDB;Integrated Security=True" />
   </connectionStrings>
   ```

   Prefer `Integrated Security=True` over a username and password so no credentials are
   committed to source control.
4. Build and run (F5).

## Status

Built as a college project. Not actively maintained — kept public as a reference for the
MVC + repository-pattern structure and the custom membership provider implementation.
