<template>
  <div>
    <div id="top">
        <a href="https://github.com/nhnent/tui.calendar">
            <img src="../assets/images/img-bi.png" srcset="../assets/images/img-bi@2x.png 2x,../assets/images/img-bi@3x.png 3x">
        </a>
    </div>
    <div id="lnb">
        <div class="lnb-new-schedule">
            <button id="btn-new-schedule" type="button" class="btn btn-default btn-block lnb-new-schedule-btn" data-toggle="modal">
                New schedule</button>
        </div>
        <div id="lnb-calendars" class="lnb-calendars">
            <div>
                <div class="lnb-calendars-item">
                    <label>
                        <input class="tui-full-calendar-checkbox-square" type="checkbox" value="all" checked>
                        <span></span>
                        <strong>View all</strong>
                    </label>
                </div>
            </div>
            <div id="calendarList" class="lnb-calendars-d1">
            </div>
        </div>
        <div class="lnb-footer">
            © NHN Entertainment Corp.
        </div>
    </div>
    <div id="right">
      <div id="menu">
        <span class="dropdown">
            <button id="dropdownMenu-calendarType" class="btn btn-default btn-sm dropdown-toggle" type="button" data-toggle="dropdown"
                aria-haspopup="true" aria-expanded="true">
                <i id="calendarTypeIcon" class="calendar-icon ic_view_month" style="margin-right: 4px;"></i>
                <span id="calendarTypeName">Dropdown</span>&nbsp;
                <i class="calendar-icon tui-full-calendar-dropdown-arrow"></i>
            </button>
            <ul class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu-calendarType">
                <li role="presentation">
                    <a class="dropdown-menu-title" role="menuitem" data-action="toggle-daily">
                        <i class="calendar-icon ic_view_day"></i>Daily
                    </a>
                </li>
                <li role="presentation">
                    <a class="dropdown-menu-title" role="menuitem" data-action="toggle-weekly">
                        <i class="calendar-icon ic_view_week"></i>Weekly
                    </a>
                </li>
                <li role="presentation">
                    <a class="dropdown-menu-title" role="menuitem" data-action="toggle-monthly">
                        <i class="calendar-icon ic_view_month"></i>Month
                    </a>
                </li>
                <li role="presentation">
                    <a class="dropdown-menu-title" role="menuitem" data-action="toggle-weeks2">
                        <i class="calendar-icon ic_view_week"></i>2 weeks
                    </a>
                </li>
                <li role="presentation">
                    <a class="dropdown-menu-title" role="menuitem" data-action="toggle-weeks3">
                        <i class="calendar-icon ic_view_week"></i>3 weeks
                    </a>
                </li>
                <li role="presentation" class="dropdown-divider"></li>
                <li role="presentation">
                    <a role="menuitem" data-action="toggle-workweek">
                        <input type="checkbox" class="tui-full-calendar-checkbox-square" value="toggle-workweek" checked>
                        <span class="checkbox-title"></span>Show weekends
                    </a>
                </li>
                <li role="presentation">
                    <a role="menuitem" data-action="toggle-start-day-1">
                        <input type="checkbox" class="tui-full-calendar-checkbox-square" value="toggle-start-day-1">
                        <span class="checkbox-title"></span>Start Week on Monday
                    </a>
                </li>
                <li role="presentation">
                    <a role="menuitem" data-action="toggle-narrow-weekend">
                        <input type="checkbox" class="tui-full-calendar-checkbox-square" value="toggle-narrow-weekend">
                        <span class="checkbox-title"></span>Narrower than weekdays
                    </a>
                </li>
            </ul>
        </span>
        <span id="menu-navi">
            <button type="button" class="btn btn-default btn-sm move-today" data-action="move-today">Today</button>
            <button type="button" class="btn btn-default btn-sm move-day" data-action="move-prev">
                <i class="calendar-icon ic-arrow-line-left" data-action="move-prev"></i>
            </button>
            <button type="button" class="btn btn-default btn-sm move-day" data-action="move-next">
                <i class="calendar-icon ic-arrow-line-right" data-action="move-next"></i>
            </button>
        </span>
        <span id="renderRange" class="render-range"></span>
      </div>
      <div id="calendar"></div>
    </div>
  </div>
</template>

<script>
import '../css/bootstrap.css';
import 'tui-time-picker/dist/tui-time-picker.css';
import 'tui-date-picker/dist/tui-date-picker.css';
import 'tui-calendar/dist/tui-calendar.min.css';
import '../css/default.css';
import '../css/icons.css';

import '../js/bootstrap.min';
//import 'tui-code-snippet';
import 'tui-time-picker'
import 'tui-date-picker'
import moment from 'moment'
import 'chance'
import Calendar from 'tui-calendar'

export default {
  name: 'Schedule',
  mounted: function () {

    var CalendarList = [];

    function CalendarInfo() {
        this.id = null;
        this.name = null;
        this.checked = true;
        this.color = null;
        this.bgColor = null;
        this.borderColor = null;
    }

    function addCalendar(calendar) {
        CalendarList.push(calendar);
    }

    function findCalendar(id) {
        var found;

        CalendarList.forEach(function(calendar) {
            if (calendar.id === id) {
                found = calendar;
            }
        });

        return found;
    }

    function hexToRGBA(hex) {
        var radix = 16;
        var r = parseInt(hex.slice(1, 3), radix),
            g = parseInt(hex.slice(3, 5), radix),
            b = parseInt(hex.slice(5, 7), radix),
            a = parseInt(hex.slice(7, 9), radix) / 255 || 1;
        var rgba = 'rgba(' + r + ', ' + g + ', ' + b + ', ' + a + ')';

        return rgba;
    }

    (function() {
        var calendar;
        var id = 0;

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'My Calendar';
        calendar.color = '#ffffff';
        calendar.bgColor = '#9e5fff';
        calendar.dragBgColor = '#9e5fff';
        calendar.borderColor = '#9e5fff';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'Company';
        calendar.color = '#ffffff';
        calendar.bgColor = '#00a9ff';
        calendar.dragBgColor = '#00a9ff';
        calendar.borderColor = '#00a9ff';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'Family';
        calendar.color = '#ffffff';
        calendar.bgColor = '#ff5583';
        calendar.dragBgColor = '#ff5583';
        calendar.borderColor = '#ff5583';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'Friend';
        calendar.color = '#ffffff';
        calendar.bgColor = '#03bd9e';
        calendar.dragBgColor = '#03bd9e';
        calendar.borderColor = '#03bd9e';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'Travel';
        calendar.color = '#ffffff';
        calendar.bgColor = '#bbdc00';
        calendar.dragBgColor = '#bbdc00';
        calendar.borderColor = '#bbdc00';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'etc';
        calendar.color = '#ffffff';
        calendar.bgColor = '#9d9d9d';
        calendar.dragBgColor = '#9d9d9d';
        calendar.borderColor = '#9d9d9d';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'Birthdays';
        calendar.color = '#ffffff';
        calendar.bgColor = '#ffbb3b';
        calendar.dragBgColor = '#ffbb3b';
        calendar.borderColor = '#ffbb3b';
        addCalendar(calendar);

        calendar = new CalendarInfo();
        id += 1;
        calendar.id = String(id);
        calendar.name = 'National Holidays';
        calendar.color = '#ffffff';
        calendar.bgColor = '#ff4040';
        calendar.dragBgColor = '#ff4040';
        calendar.borderColor = '#ff4040';
        addCalendar(calendar);
    })();


    /**/

    var ScheduleList = [];

    var SCHEDULE_CATEGORY = [
        'milestone',
        'task'
    ];

    function ScheduleInfo() {
        this.id = null;
        this.calendarId = null;

        this.title = null;
        this.isAllday = false;
        this.start = null;
        this.end = null;
        this.category = '';
        this.dueDateClass = '';

        this.color = null;
        this.bgColor = null;
        this.dragBgColor = null;
        this.borderColor = null;
        this.customStyle = '';

        this.isFocused = false;
        this.isPending = false;
        this.isVisible = true;
        this.isReadOnly = false;

        this.raw = {
            memo: '',
            hasToOrCc: false,
            hasRecurrenceRule: false,
            location: null,
            class: 'public', // or 'private'
            creator: {
                name: '',
                avatar: '',
                company: '',
                email: '',
                phone: ''
            }
        };
    }

    function generateTime(schedule, renderStart, renderEnd) {
        var baseDate = new Date(renderStart);
        var singleday = chance.bool({likelihood: 70});
        var startDate = moment(renderStart.getTime())
        var endDate = moment(renderEnd.getTime());
        var diffDate = endDate.diff(startDate, 'days');


        schedule.isAllday = chance.bool({likelihood: 30});
        if (schedule.isAllday) {
            schedule.category = 'allday';
        } else if (chance.bool({likelihood: 30})) {
            schedule.category = SCHEDULE_CATEGORY[chance.integer({min: 0, max: 1})];
            if (schedule.category === SCHEDULE_CATEGORY[1]) {
                schedule.dueDateClass = 'morning';
            }
        } else {
            schedule.category = 'time';
        }

        startDate.add(chance.integer({min: 0, max: diffDate}), 'days');
        startDate.hours(chance.integer({min: 0, max: 23}))
        startDate.minutes(chance.bool() ? 0 : 30);
        schedule.start = startDate.toDate();

        endDate = moment(startDate);
        if (schedule.isAllday) {
            endDate.add(chance.integer({min: 0, max: 3}), 'days');
        }

        schedule.end = endDate
            .add(chance.integer({min: 1, max: 4}), 'hour')
            .toDate();
    }

    function generateRandomSchedule(calendar, renderStart, renderEnd) {
        var schedule = new ScheduleInfo();

        schedule.id = chance.guid();
        schedule.calendarId = calendar.id;

        schedule.title = chance.sentence({words: 3});
        schedule.isReadOnly = chance.bool({likelihood: 20});
        generateTime(schedule, renderStart, renderEnd);

        schedule.isPrivate = chance.bool({likelihood: 10});
        schedule.location = chance.address();
        schedule.attendees = chance.bool({likelihood: 70}) ? ['anyone']: [];
        schedule.recurrenceRule = chance.bool({likelihood: 20});

        schedule.color = calendar.color;
        schedule.bgColor = calendar.bgColor;
        schedule.dragBgColor = calendar.dragBgColor;
        schedule.borderColor = calendar.borderColor;

        if (schedule.category === 'milestone') {
            schedule.color = schedule.bgColor;
            schedule.bgColor = 'transparent';
            schedule.dragBgColor = 'transparent';
            schedule.borderColor = 'transparent';
        }

        schedule.raw.memo = chance.sentence();
        schedule.raw.creator.name = chance.name();
        schedule.raw.creator.avatar = chance.avatar();
        schedule.raw.creator.company = chance.company();
        schedule.raw.creator.email = chance.email();
        schedule.raw.creator.phone = chance.phone();

        ScheduleList.push(schedule);
    }

    function generateSchedule(viewName, renderStart, renderEnd) {
        ScheduleList = [];
        CalendarList.forEach(function(calendar) {
            var i = 0, length = 10;
            if (viewName === 'month') {
                length = 15;
            } else if (viewName === 'day') {
                length = 4;
            }
            for (; i < length; i += 1) {
                generateRandomSchedule(calendar, renderStart, renderEnd);
            }
        });
    }


    /**/

      var cal, resizeThrottled;
      var useCreationPopup = true;
      var useDetailPopup = true;
      var datePicker, selectedCalendar;

      cal = new Calendar('#calendar', {
          defaultView: 'month',
          useCreationPopup: useCreationPopup,
          useDetailPopup: useDetailPopup,
          calendars: CalendarList,
          template: {
              milestone: function(model) {
                  return '<span class="calendar-font-icon ic-milestone-b"></span> <span style="background-color: ' + model.bgColor + '">' + model.title + '</span>';
              },
              allday: function(schedule) {
                  return getTimeTemplate(schedule, true);
              },
              time: function(schedule) {
                  return getTimeTemplate(schedule, false);
              }
          }
      });

      // event handlers
      cal.on({
          'clickSchedule': function(e) {
              console.log('clickSchedule', e);
          },
          'clickDayname': function(date) {
              console.log('clickDayname', date);
          },
          'beforeCreateSchedule': function(e) {
              console.log('beforeCreateSchedule', e);
              saveNewSchedule(e);
          },
          'beforeUpdateSchedule': function(e) {
              console.log('beforeUpdateSchedule', e);
              e.schedule.start = e.start;
              e.schedule.end = e.end;
              cal.updateSchedule(e.schedule.id, e.schedule.calendarId, e.schedule);
          },
          'beforeDeleteSchedule': function(e) {
              console.log('beforeDeleteSchedule', e);
              cal.deleteSchedule(e.schedule.id, e.schedule.calendarId);
          },
          'afterRenderSchedule': function(e) {
              var schedule = e.schedule;
              // var element = cal.getElement(schedule.id, schedule.calendarId);
              // console.log('afterRenderSchedule', element);
          }
      });

      /**
       * Get time template for time and all-day
       * @param {Schedule} schedule - schedule
       * @param {boolean} isAllDay - isAllDay or hasMultiDates
       * @returns {string}
       */
      function getTimeTemplate(schedule, isAllDay) {
          var html = [];
          var start = moment(schedule.start.toUTCString());
          if (!isAllDay) {
              html.push('<strong>' + start.format('HH:mm') + '</strong> ');
          }
          if (schedule.isPrivate) {
              html.push('<span class="calendar-font-icon ic-lock-b"></span>');
              html.push(' Private');
          } else {
              if (schedule.isReadOnly) {
                  html.push('<span class="calendar-font-icon ic-readonly-b"></span>');
              } else if (schedule.recurrenceRule) {
                  html.push('<span class="calendar-font-icon ic-repeat-b"></span>');
              } else if (schedule.attendees.length) {
                  html.push('<span class="calendar-font-icon ic-user-b"></span>');
              } else if (schedule.location) {
                  html.push('<span class="calendar-font-icon ic-location-b"></span>');
              }
              html.push(' ' + schedule.title);
          }

          return html.join('');
      }

      /**
       * A listener for click the menu
       * @param {Event} e - click event
       */
      function onClickMenu(e) {
          var target = $(e.target).closest('a[role="menuitem"]')[0];
          var action = getDataAction(target);
          var options = cal.getOptions();
          var viewName = '';

          console.log(target);
          console.log(action);
          switch (action) {
              case 'toggle-daily':
                  viewName = 'day';
                  break;
              case 'toggle-weekly':
                  viewName = 'week';
                  break;
              case 'toggle-monthly':
                  options.month.visibleWeeksCount = 0;
                  viewName = 'month';
                  break;
              case 'toggle-weeks2':
                  options.month.visibleWeeksCount = 2;
                  viewName = 'month';
                  break;
              case 'toggle-weeks3':
                  options.month.visibleWeeksCount = 3;
                  viewName = 'month';
                  break;
              case 'toggle-narrow-weekend':
                  options.month.narrowWeekend = !options.month.narrowWeekend;
                  options.week.narrowWeekend = !options.week.narrowWeekend;
                  viewName = cal.getViewName();

                  target.querySelector('input').checked = options.month.narrowWeekend;
                  break;
              case 'toggle-start-day-1':
                  options.month.startDayOfWeek = options.month.startDayOfWeek ? 0 : 1;
                  options.week.startDayOfWeek = options.week.startDayOfWeek ? 0 : 1;
                  viewName = cal.getViewName();

                  target.querySelector('input').checked = options.month.startDayOfWeek;
                  break;
              case 'toggle-workweek':
                  options.month.workweek = !options.month.workweek;
                  options.week.workweek = !options.week.workweek;
                  viewName = cal.getViewName();

                  target.querySelector('input').checked = !options.month.workweek;
                  break;
              default:
                  break;
          }

          cal.setOptions(options, true);
          cal.changeView(viewName, true);

          setDropdownCalendarType();
          setRenderRangeText();
          setSchedules();
      }

      function onClickNavi(e) {
          var action = getDataAction(e.target);

          switch (action) {
              case 'move-prev':
                  cal.prev();
                  break;
              case 'move-next':
                  cal.next();
                  break;
              case 'move-today':
                  cal.today();
                  break;
              default:
                  return;
          }

          setRenderRangeText();
          setSchedules();
      }

      function onNewSchedule() {
          var title = $('#new-schedule-title').val();
          var location = $('#new-schedule-location').val();
          var isAllDay = document.getElementById('new-schedule-allday').checked;
          var start = datePicker.getStartDate();
          var end = datePicker.getEndDate();
          var calendar = selectedCalendar ? selectedCalendar : CalendarList[0];

          if (!title) {
              return;
          }

          cal.createSchedules([{
              id: String(chance.guid()),
              calendarId: calendar.id,
              title: title,
              isAllDay: isAllDay,
              start: start,
              end: end,
              category: isAllDay ? 'allday' : 'time',
              dueDateClass: '',
              color: calendar.color,
              bgColor: calendar.bgColor,
              dragBgColor: calendar.bgColor,
              borderColor: calendar.borderColor,
              raw: {
                  location: location
              },
              state: 'Busy'
          }]);

          $('#modal-new-schedule').modal('hide');
      }

      function onChangeNewScheduleCalendar(e) {
          var target = $(e.target).closest('a[role="menuitem"]')[0];
          var calendarId = getDataAction(target);
          changeNewScheduleCalendar(calendarId);
      }

      function changeNewScheduleCalendar(calendarId) {
          var calendarNameElement = document.getElementById('calendarName');
          var calendar = findCalendar(calendarId);
          var html = [];

          html.push('<span class="calendar-bar" style="background-color: ' + calendar.bgColor + '; border-color:' + calendar.borderColor + ';"></span>');
          html.push('<span class="calendar-name">' + calendar.name + '</span>');

          calendarNameElement.innerHTML = html.join('');

          selectedCalendar = calendar;
      }

      function createNewSchedule(event) {
          var start = event.start ? new Date(event.start.getTime()) : new Date();
          var end = event.end ? new Date(event.end.getTime()) : moment().add(1, 'hours').toDate();

          if (useCreationPopup) {
              cal.openCreationPopup({
                  start: start,
                  end: end
              });
          }
      }
      function saveNewSchedule(scheduleData) {
          var calendar = scheduleData.calendar || findCalendar(scheduleData.calendarId);
          var schedule = {
              id: String(chance.guid()),
              title: scheduleData.title,
              isAllDay: scheduleData.isAllDay,
              start: scheduleData.start,
              end: scheduleData.end,
              category: scheduleData.isAllDay ? 'allday' : 'time',
              dueDateClass: '',
              color: calendar.color,
              bgColor: calendar.bgColor,
              dragBgColor: calendar.bgColor,
              borderColor: calendar.borderColor,
              raw: {
                  'class': scheduleData.raw['class'],
                  location: scheduleData.raw.location
              },
              state: scheduleData.state
          };
          if (calendar) {
              schedule.calendarId = calendar.id;
              schedule.color = calendar.color;
              schedule.bgColor = calendar.bgColor;
              schedule.borderColor = calendar.borderColor;
          }

          cal.createSchedules([schedule]);

          refreshScheduleVisibility();
      }

      function onChangeCalendars(e) {
          var calendarId = e.target.value;
          var checked = e.target.checked;
          var viewAll = document.querySelector('.lnb-calendars-item input');
          var calendarElements = Array.prototype.slice.call(document.querySelectorAll('#calendarList input'));
          var allCheckedCalendars = true;

          if (calendarId === 'all') {
              allCheckedCalendars = checked;

              calendarElements.forEach(function(input) {
                  var span = input.parentNode;
                  input.checked = checked;
                  span.style.backgroundColor = checked ? span.style.borderColor : 'transparent';
              });

              CalendarList.forEach(function(calendar) {
                  calendar.checked = checked;
              });
          } else {
              findCalendar(calendarId).checked = checked;

              allCheckedCalendars = calendarElements.every(function(input) {
                  return input.checked;
              });

              if (allCheckedCalendars) {
                  viewAll.checked = true;
              } else {
                  viewAll.checked = false;
              }
          }

          refreshScheduleVisibility();
      }

      function refreshScheduleVisibility() {
          var calendarElements = Array.prototype.slice.call(document.querySelectorAll('#calendarList input'));

          CalendarList.forEach(function(calendar) {
              cal.toggleSchedules(calendar.id, !calendar.checked, false);
          });

          cal.render(true);

          calendarElements.forEach(function(input) {
              var span = input.nextElementSibling;
              span.style.backgroundColor = input.checked ? span.style.borderColor : 'transparent';
          });
      }

      function setDropdownCalendarType() {
          var calendarTypeName = document.getElementById('calendarTypeName');
          var calendarTypeIcon = document.getElementById('calendarTypeIcon');
          var options = cal.getOptions();
          var type = cal.getViewName();
          var iconClassName;

          if (type === 'day') {
              type = 'Daily';
              iconClassName = 'calendar-icon ic_view_day';
          } else if (type === 'week') {
              type = 'Weekly';
              iconClassName = 'calendar-icon ic_view_week';
          } else if (options.month.visibleWeeksCount === 2) {
              type = '2 weeks';
              iconClassName = 'calendar-icon ic_view_week';
          } else if (options.month.visibleWeeksCount === 3) {
              type = '3 weeks';
              iconClassName = 'calendar-icon ic_view_week';
          } else {
              type = 'Monthly';
              iconClassName = 'calendar-icon ic_view_month';
          }

          calendarTypeName.innerHTML = type;
          calendarTypeIcon.className = iconClassName;
      }

      function setRenderRangeText() {
          var renderRange = document.getElementById('renderRange');
          var options = cal.getOptions();
          var viewName = cal.getViewName();
          var html = [];
          if (viewName === 'day') {
              html.push(moment(cal.getDate().getTime()).format('YYYY.MM.DD'));
          } else if (viewName === 'month' &&
              (!options.month.visibleWeeksCount || options.month.visibleWeeksCount > 4)) {
              html.push(moment(cal.getDate().getTime()).format('YYYY.MM'));
          } else {
              html.push(moment(cal.getDateRangeStart().getTime()).format('YYYY.MM.DD'));
              html.push(' ~ ');
              html.push(moment(cal.getDateRangeEnd().getTime()).format(' MM.DD'));
          }
          renderRange.innerHTML = html.join('');
      }

      function setSchedules() {
          cal.clear();
          generateSchedule(cal.getViewName(), cal.getDateRangeStart(), cal.getDateRangeEnd());
          cal.createSchedules(ScheduleList);
          refreshScheduleVisibility();
      }

      function setEventListener() {
          $('#menu-navi').on('click', onClickNavi);
          $('.dropdown-menu a[role="menuitem"]').on('click', onClickMenu);
          $('#lnb-calendars').on('change', onChangeCalendars);

          $('#btn-save-schedule').on('click', onNewSchedule);
          $('#btn-new-schedule').on('click', createNewSchedule);

          $('#dropdownMenu-calendars-list').on('click', onChangeNewScheduleCalendar);

          window.addEventListener('resize', resizeThrottled);
      }

      function getDataAction(target) {
          return target.dataset ? target.dataset.action : target.getAttribute('data-action');
      }

      /*resizeThrottled = tui.util.throttle(function() {
          cal.render();
      }, 50);*/

      window.cal = cal;

      setDropdownCalendarType();
      setRenderRangeText();
      setSchedules();
      setEventListener();

    // set calendars
    (function() {
        var calendarList = document.getElementById('calendarList');
        var html = [];
        CalendarList.forEach(function(calendar) {
            html.push('<div class="lnb-calendars-item"><label>' +
                '<input type="checkbox" class="tui-full-calendar-checkbox-round" value="' + calendar.id + '" checked>' +
                '<span style="border-color: ' + calendar.borderColor + '; background-color: ' + calendar.borderColor + ';"></span>' +
                '<span>' + calendar.name + '</span>' +
                '</label></div>'
            );
        });
        calendarList.innerHTML = html.join('\n');
    })();
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
