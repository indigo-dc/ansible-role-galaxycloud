#!/usr/bin/env python
# ELIXIR-ITALY
# INDIGO-DataCloud
# IBIOM-CNR
#
# Contributors:
# author: Tangaro Marco
# email: ma.tangaro@ibiom.cnr.it
#
# Dependencies:
# uwsgi
# lsof

__version__ = "0.0.1"

# Imports
import socket
import errno
import json
import os
import signal
import time
import subprocess
import platform

try:
  import ConfigParser
except ImportError:
  import configparser

# Log config
import logging
logging.basicConfig(filename='/var/log/galaxy/galaxyctl.log', format='%(levelname)s %(asctime)s %(message)s', level=logging.DEBUG)


####################################

class bcolors:
  HEADER = '\033[95m'
  OKBLUE = '\033[94m'
  OKGREEN = '\033[92m'
  WARNING = '\033[93m'
  FAIL = '\033[91m'
  ENDC = '\033[0m'
  BOLD = '\033[1m'
  UNDERLINE = '\033[4m'

  status_ok = '[ ' + OKGREEN + 'OK' + ENDC + ' ]'
  status_stop = '[ STOP ]'
  status_fail = '[ ' + FAIL + 'FAIL' + ENDC + ' ]'


####################################

class DetectGalaxyCommands:
  def __init__(self,init_system):
    self.init = init_system
    self.os, self.version, self.codename = platform.dist()

  #______________________________________
  def get_startup_command(self):
    if self.init == 'supervisord':
      if self.os == 'centos': return 'supervisord -c /etc/supervisord.conf'
      if self.os == 'Ubuntu': return 'supervisord -c /etc/supervisor/supervisord.conf'
    elif self.init == 'init':
      if self.os == 'centos': return 'systemctl start galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'xenial': return 'systemctl start galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'trusty': return 'service galaxy start'

  #______________________________________
  def get_stop_command(self):
    if self.init == 'supervisord': return 'supervisorctl stop galaxy:'
    elif self.init == 'init':
      if self.os == 'centos': return 'systemctl stop galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'xenial': return 'systemctl stop galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'trusty': return 'service galaxy stop'

  #______________________________________
  def get_start_command(self):
    if self.init == 'supervisord': return 'supervisorctl start galaxy:'
    elif self.init == 'init':
      if self.os == 'centos': return 'systemctl start galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'xenial': return 'systemctl start galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'trusty': return 'service galaxy start'

  #______________________________________
  def get_status_command(self):
    if self.init == 'supervisord': return 'supervisorctl status galaxy:'
    elif self.init == 'init':
      if self.os == 'centos': return 'systemctl status galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'xenial': return 'systemctl status galaxy.service'
      if self.os == 'Ubuntu' and self.codename == 'trusty': return 'service galaxy status'

  #______________________________________
  def get_init(self): return self.init
  def get_os(self): return self.os
  def get_version(self): return self.version
  def get_codename(self): return self.codename

  #______________________________________
  def set_init(self, init): self.init = init
  def set_os(self, os): self.os = os
  def set_version(self, version): self.version = version
  def set_codename(self, codename): self.codename = codename

####################################

class UwsgiSocket:
  def __init__(self, server=None, port=None, timeout=None, fname=None):
    self.server = server
    self.port = port
    self.timeout = timeout

    if fname is not None:
      self.fname = fname

      configParser = ConfigParser.RawConfigParser()
      configParser.readfp(open(fname))
      configParser.read(fname)

      section = 'uwsgi'
      option = 'socket'

      if configParser.has_option(section, option):
        self.par = configParser.get(section , option)
        self.server = self.par.split(':')[0]
        self.port = int(self.par.split(':')[1])
      else:
        raise Exception('No [uwsgi] section in %s' % fname)

  #______________________________________
  def get_server(self): return self.server
  def get_port(self): return self.port

  #______________________________________
  def set_server(self, server): self.server = server
  def set_port(self, port): self.port = port

  #______________________________________
  def get_uwsgi_master_pid(self):
    command = 'lsof -t -i :%s' % self.port
    proc = subprocess.Popen( args=command, shell=True,  stdout=subprocess.PIPE, stderr=subprocess.PIPE )
    communicateRes = proc.communicate()
    stdOutValue, stdErrValue = communicateRes
    status = proc.wait()
    return stdOutValue, stdErrValue, status

####################################

class UwsgiStatsServer:
  def __init__(self, server=None, port=None, timeout=None, fname=None):
    self.server = server
    self.port = port
    self.timeout = timeout

    if fname is not None:
      self.fname = fname

      configParser = ConfigParser.RawConfigParser()
      configParser.readfp(open(fname))
      configParser.read(fname)

      section = 'uwsgi'
      option = 'stats'

      if configParser.has_option(section, option):
        self.par = configParser.get(section , option)
        self.server = self.par.split(':')[0]
        self.port = int(self.par.split(':')[1])
      else:
        raise Exception('No [uwsgi] section in %s' % fname)

  #______________________________________
  def GetUwsgiStatsServer(self):

    logging.debug('[galaxyctl_libs UwsgiStatsServer] GetUwsgiStatsServer()')
    logging.debug('[galaxyctl_libs UwsgiStatsServer] Waiting Galaxy stats server')
    logging.debug('[galaxyctl_libs UwsgiStatsServer] Timeout: %s' % self.timeout)

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    if self.timeout:
        from time import time as now
        # time module is needed to calc timeout shared between two exceptions
        end = now() + self.timeout

    while True:
      try:
        if self.timeout:
          next_timeout = end - now()
          if next_timeout < 0:
            return False
          else:
            s.settimeout(next_timeout)

        s.connect((self.server, self.port))

      except socket.timeout, err:
        # this exception occurs only if timeout is set
        if self.timeout:
          return False

      except socket.error, err:
        # catch timeout exception from underlying network library
        # this one is different from socket.timeout
        if type(err.args) != tuple or err[0] != errno.ETIMEDOUT:
          pass
      else:
        logging.debug('[galaxyctl_libs UwsgiStatsServer] Stats server enabled on: %s:%s' % (self.server, self.port))
        return s

  #______________________________________
  def GetStatsJson(self):
    reply = self.GetUwsgiStatsServer().recv(131072)
    return reply

  #______________________________________
  # Returns params from *.ini file
  def ReadUwsgiIniFile(self, fname, section, option):
    configParser = ConfigParser.RawConfigParser()
    configParser.readfp(open(fname))
    configParser.read(fname)

    self.par = 0

    if configParser.has_option(section, option):
      self.par = configParser.get(section , option)
    else:
      logging.debug('[galaxyctl_libs UwsgiStatsServer] No [%s] section in %s' % (section, fname))
      return False

    return self.par

  #______________________________________
  # Check if uwsgi workers accept requests or not
  def CheckUwsgiWorkers(self, fname):

    if fname is not None:
      self.fname = fname

    logging.debug('[galaxyctl_libs UwsgiStatsServer] CheckUwsgiWorkers(fname=%s)' % fname)

    uwsgi_processes = self.ReadUwsgiIniFile(fname, 'uwsgi', 'processes')
    if uwsgi_processes is False:
      return False
    else:
      logging.debug('[galaxyctl_libs UwsgiStatsServer] UWSGI processes: %s' % uwsgi_processes)

    stats_json = self.GetStatsJson()
    stats_dictionary = json.loads(stats_json)

    logging.debug('[galaxyctl_libs UwsgiStatsServer] Check uWSGI workers status')

    for workers in stats_dictionary['workers']:
      workers_id = workers.get('id')
      workers_status = workers.get('accepting')
      workers_pid = workers.get('pid')
      logging.debug('[galaxyctl_libs UwsgiStatsServer] Worker pid: %s' % workers_pid)
      logging.debug('[galaxyctl_libs UwsgiStatsServer] Worker status: %s' % workers_status)
      if workers_status == 1:
        return True

    logging.debug('[galaxyctl_libs UwsgiStatsServer] No uWSGI workers accepting requests')
    return False

  #______________________________________
  def GetBusyList(self, fname=None):

    if fname is not None:
      self.fname = fname

    logging.debug('[galaxyctl_libs UwsgiStatsServer] GetBusyList(fname=%s)' % fname)

    busy_list = []

    # Get uwsgi workers number
    uwsgi_processes = self.ReadUwsgiIniFile(self.fname, 'uwsgi', 'processes')

    logging.debug('[galaxyctl_libs UwsgiStatsServer] Wait Galaxy stats server on %s:%s' % (self.server, self.port))
    stats = self.GetUwsgiStatsServer()

    if stats:
      stats_json = self.GetStatsJson()
      stats_dictionary = json.loads(stats_json)

      for workers in stats_dictionary['workers']:
        workers_id = workers.get('id')
        workers_status = workers.get('accepting')
        workers_pid = workers.get('pid')
        if workers_status == 0:
          busy_list.append(workers_pid)

      logging.debug('[galaxyctl_libs UwsgiStatsServer] Busy list: %s' % busy_list)
      return busy_list


####################################
# This class reads luksctl config file

class LUKSCtl:
  def __init__(self, fname):

    self.fname = fname

    configParser = ConfigParser.RawConfigParser()
    configParser.readfp(open(fname))
    configParser.read(fname)

    self.cipher_algorithm = configParser.get('luks', 'cipher_algorithm')
    self.hash_algorithm = configParser.get('luks', 'hash_algorithm')
    self.keysize = configParser.get('luks', 'keysize')
    self.device = configParser.get('luks', 'device')
    self.uuid = configParser.get('luks', 'uuid')
    self.cryptdev = configParser.get('luks', 'cryptdev')
    self.mapper = configParser.get('luks', 'mapper')
    self.mountpoint = configParser.get('luks', 'mountpoint')
    self.filesystem = configParser.get('luks', 'filesystem')

  #______________________________________
  # getter
  def get_cipher_algorithm(self): return self.cipher_algorithm
  def get_hash_algorithm(self): return self.hash_algorithm
  def get_keysize(self): return self.keysize
  def get_device(self): return self.device
  def get_uuid(self): return self.uuid
  def get_cryptdev(self): return self.cryptdev
  def get_mapper(self): return self.mapper
  def get_mountpoint(self): return self.mountpoint
  def get_filesystem(self): return self.filesystem

  #______________________________________
  # setter
  def set_cipher_algorithm(self, cipher_algorithm): self.cipher_algorithm = cipher_algorithm
  def set_hash_algorithm(self, hash_algorithm): self.hash_algorithm = hash_algorithm
  def set_keysize(self, keysize): keysize = keysize
  def set_device(self, device): self.device = device
  def set_uuid(self, uuid): self.uuid = uuid
  def set_cryptdev(self, cryptdev): self.cryptdev = cryptdev
  def set_mapper(self, mapper): self.mapper = mapper
  def set_mountpoint(self, mountpoint): self.mountpoint = mountpoint
  def set_filesystem(self, filesystem): self.filesystem = filesystem


####################################
# This class reads onedatactl config file

class OneDataCtl:
  def __init__(self, fname, section):

    self.fname = fname
    self.section = section
    self.mountpoint = None
    self.provider = None
    self.token = None
    self.insecure = False
    self.nonempty = False
    self.uid = 4001
    self.gid = 4001

    configParser = ConfigParser.RawConfigParser()
    configParser.readfp(open(fname))
    configParser.read(fname)

    # mountpoint
    if configParser.has_option(self.section, 'mountpoint'):
      self.mountpoint = configParser.get(self.section, 'mountpoint')
    else:
      logging.error('[galaxyctl_libs OneDataCtl] No mountpoint in section %s.' % section)
      raise Exception('[galaxyctl_libs OneDataCtl] No mounpoint in section %s.' % section)

    # provider
    if configParser.has_option(self.section, 'provider'):
      self.provider = configParser.get(self.section, 'provider')
    else:
      logging.error('[galaxyctl_libs OneDataCtl] No provider specified section in %s.' % section)
      raise Exception('[galaxyctl_libs OneDataCtl] No provider in section %s.' % section)

    # token
    if configParser.has_option(self.section, 'token'):
      self.token = configParser.get(self.section, 'token')
    else:
      logging.error('[galaxyctl_libs OneDataCtl] No token specified section in %s.' % section)
      raise Exception('[galaxyctl_libs OneDataCtl] No token in section %s.' % section)

    if configParser.has_option(self.section, 'insecure'): self.insecure = configParser.get(self.section, 'insecure')
    if configParser.has_option(self.section, 'nonempty'): self.nonempty = configParser.get(self.section, 'nonempty')
    if configParser.has_option(self.section, 'uid'): self.nonempty = configParser.get(self.section, 'uid')
    if configParser.has_option(self.section, 'gid'): self.nonempty = configParser.get(self.section, 'gid')

  #______________________________________
  # getter
  def get_mountpoint(self): return self.mountpoint
  def get_provider(self): return self.provider
  def get_token(self): return self.token
  def get_insecure(self): return self.insecure
  def get_nonempty(self): return self.nonempty
  def get_gid(self): return self.gid
  def get_uid(self): return self.uid

  #______________________________________
  # setter
  def set_mountpoint(self, mountpoint): self.mountpoint = mountpoint
  def set_provider(self, provider): self.provider = provider
  def set_token(self, token): self.token = token
  def set_insecure(self, insecure): self.insecure = insecure
  def set_nonempty(self, nonempty): self.nonempty = nonempty
  def set_uid(self, uid): self.uid = uid
  def set_gid(self, gid): self.gid = gid

  #______________________________________
  # Mount space
  def mount_space(self):
    command = 'oneclient -H %s -t %s %s' % ( self.provider, self.token, self.mountpoint)
    if self.insecure == 'True': command += ' --insecure'
    if self.nonempty == 'True': command += ' -o nonempty'

    logging.debug('[galaxyctl_libs OneDataCtl] %s' % str(command))
    subprocess.call( command, shell=True, preexec_fn=self.demote(self.uid, self.gid) )

  #______________________________________
  # Umount space
  def umount_space(self):
    command = 'fusermount -u %s' % (self.mountpoint)
    logging.debug('[galaxyctl_libs OneDataCtl] %s' % str(command))
    subprocess.call( command, shell=True, preexec_fn=self.demote(self.uid, self.gid) )

  #______________________________________
  def demote(self, user_uid, user_gid):
     logging.debug('[galaxyctl_libs OneDataCtl] Starting demotion: uid, gid = %d, %d' % (os.getuid(), os.getgid()) )
     os.setgid(user_gid)
     os.setuid(user_uid)
     logging.debug('[galaxyctl_libs OneDataCtl] Finished demotion: uid, gid = %d, %d' % (os.getuid(), os.getgid()) )
